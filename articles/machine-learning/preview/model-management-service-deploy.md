---
title: "Azure Machine Learning 모델 관리 웹 서비스 배포 | Microsoft Docs"
description: "이 문서에서는 Azure Machine Learning 모델 관리를 사용하여 기계 학습 모델을 배포하는 단계에 대해 설명합니다."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 0ebf8964bc565e5d4f95fb1ad3edbc16f841851c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2017
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>웹 서비스로 Machine Learning 웹 학습 모델 배포

Azure Machine Learning 모델 관리는 모델을 컨테이너화된 Docker 기반 웹 서비스로 배포하는 인터페이스를 제공합니다. Spark, Microsoft CNTK(Cognitive Toolkit), Keras, Tensorflow 및 Python과 같은 프레임워크를 사용하여 만든 모델을 배포할 수 있습니다. 

이 문서에서는 Azure Machine Learning 모델 관리 CLI(명령줄 인터페이스)를 사용하여 모델을 웹 서비스로 배포하는 단계에 대해 설명합니다.

## <a name="deploying-web-services"></a>웹 서비스 배포
CLI를 사용하여 웹 서비스를 배포하여 로컬 컴퓨터 또는 클러스터에서 실행할 수 있습니다.

로컬 배포부터 시작하는 것이 좋습니다. 먼저 모델과 코드가 작동하는지에 대한 유효성을 검사한 다음 프로덕션 규모로 사용할 클러스터에 웹 서비스를 배포합니다. 클러스터 배포를 위한 환경 설정에 대한 자세한 내용은 [모델 관리 구성](model-management-configuration.md)을 참조하세요. 

배포 단계는 다음과 같습니다.
1. 학습되고 저장된 Machine Learning 모델 사용
2. 웹 서비스의 입력 및 출력 데이터에 대한 스키마 만들기
3. Docker 기반 컨테이너 이미지 만들기
4. 웹 서비스 만들기 및 배포

### <a name="1-save-your-model"></a>1. 모델 저장
학습되고 저장된 모델 파일(예: mymodel.pkl)로 시작합니다. 파일 확장명은 모델을 학습하고 저장하는 데 사용하는 플랫폼에 따라 다릅니다. 

예를 들어 Python의 Pickle 라이브러리를 사용하여 학습된 모델을 파일에 저장할 수 있습니다. 다음은 아이리스 데이터 집합을 사용하는 [예제](http://scikit-learn.org/stable/modules/model_persistence.html)입니다.

```python
import pickle
from sklearn import datasets
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf.fit(X, y)  
saved_model = pickle.dumps(clf)
```

### <a name="2-create-a-schemajson-file"></a>2. schema.json 파일 만들기
이 단계는 옵션입니다. 

웹 서비스에 대한 입력과 출력의 유효성을 자동으로 검사하는 스키마를 만듭니다. 또한 CLI는 이 스키마를 사용하여 웹 서비스에 대한 Swagger 문서를 생성합니다.

스키마를 만들려면 다음 라이브러리를 가져옵니다.

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
그런 다음 Spark 데이터 프레임, Pandas 데이터 프레임 또는 NumPy 배열과 같은 입력 변수를 정의합니다. 다음 예제에서는 Numpy 배열을 사용합니다.

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```
다음 예제에서는 Spark 데이터 프레임을 사용합니다.

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

다음 예제에서는 PANDAS 데이터 프레임을 사용합니다.

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. score.py 파일 만들기
모델을 로드하고 모델을 사용하여 예측 결과를 반환하는 score.py 파일을 제공합니다.

이 파일에는 init와 run의 두 함수가 포함되어야 합니다.

score.py 파일의 맨 위에 다음 코드를 추가하여 모델 입력 및 예측 데이터를 수집하는 데 도움이 되는 데이터 수집 기능을 사용하도록 설정합니다.

    ```
    from azureml.datacollector import ModelDataCollector
    ```

이 기능을 사용하는 방법에 대한 자세한 내용은 [모델 데이터 수집](how-to-use-model-data-collection.md) 섹션을 확인하세요.

#### <a name="init-function"></a>init 함수
init 함수는 저장된 모델을 로드하는 데 사용됩니다.

모델을 로드하는 간단한 init 함수의 예제:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>run 함수
run 함수는 모델과 입력 데이터를 사용하여 예측을 반환합니다.

입력을 처리하고 예측 결과를 반환하는 간단한 run 함수의 예제:

```python
def run(input_df):
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. 모델 등록
다음 명령은 위의 1단계에서 만든 모델을 등록하는 데 사용됩니다.

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. 매니페스트 만들기
다음 명령은 모델의 매니페스트를 만드는 데 도움이 됩니다.

```
az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]
```
위에 표시된 명령에서 `--model-id` 또는 `-i` 인수를 사용하여 매니페스트에 이전에 등록한 모델을 추가할 수 있습니다. -i 인수를 추가하여 여러 모델을 지정할 수 있습니다.

### <a name="6-create-an-image"></a>6. 이미지 만들기 
이미지는 이전에 매니페스트를 만든 옵션을 사용하여 만들 수 있습니다. 

```
az ml image create -n [image name] -manifest-id [the manifest ID]
```

또는 단일 명령을 사용하여 매니페스트와 이미지를 만들 수 있습니다. 

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>명령 매개 변수에 대한 자세한 내용을 보려면 명령 끝에 -h를 입력하세요(예: az ml image create -h).


### <a name="7-create-and-deploy-the-web-service"></a>7. 웹 서비스 만들기 및 배포
다음 명령을 사용하여 서비스를 배포합니다.

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>단일 명령을 사용하여 이전 4단계를 수행할 수도 있습니다. 자세한 내용을 보려면 service create 명령과 함께 -h를 사용하세요.

### <a name="8-test-the-service"></a>8. 서비스 테스트
다음 명령을 사용하여 서비스를 호출하는 방법에 대한 정보를 가져옵니다.

```
az ml service usage realtime -i <service id>
```

명령 프롬프트에서 run 함수를 사용하여 서비스를 호출합니다.

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

다음 예제에서는 샘플 아이리스 웹 서비스를 호출합니다.

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>다음 단계
이제 웹 서비스를 로컬로 실행하여 테스트했으므로 대규모로 사용할 클러스터에 웹 서비스를 배포할 수 있습니다. 웹 서비스 배포를 위한 클러스터 설정에 대한 자세한 내용은 [모델 관리 구성](model-management-configuration.md)을 참조하세요. 
