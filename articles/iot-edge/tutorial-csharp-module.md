---
title: "Azure IoT Edge C# 모듈 | Microsoft Docs"
description: "C# 코드가 있는 IoT Edge 모듈을 만들어 에지 장치에 배포"
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: fb7674d8c292e7d571a94ac4625b0858a90704b3
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>C# IoT Edge 모듈을 개발하여 시뮬레이트된 장치에 배포 - 미리 보기

비즈니스 논리를 직접 IoT Edge 장치에 구현하는 코드를 배포하려면 IoT Edge 모듈을 사용할 수 있습니다. 이 자습서에서는 “[Windows][lnk-tutorial1-win] 또는 [Linux][lnk-tutorial1-lin]에서 시뮬레이트된 장치에 Azure IoT Edge 배포” 자습서에서 만든 IoT Edge 장치에서 센서 데이터를 필터링하는 IoT Edge 모듈을 만들고 배포하는 과정을 안내합니다. 이 자습서에서는 다음 방법에 대해 알아봅니다.    

> [!div class="checklist"]
> * Visual Studio Code를 사용하여 .NET 코어 2.0을 기반으로 IoT Edge 모듈을 만듭니다.
> * Visual Studio Code 및 Docker를 사용하여 docker 이미지를 만들어 레지스트리에 게시합니다. 
> * 모듈을 IoT Edge 장치에 배포합니다.
> * 생성된 데이터 보기


이 자습서에서 만든 IoT Edge 모듈은 장치에서 생성된 온도 데이터를 필터링하고, 온도가 지정된 임계값보다 높은 경우에만 메시지 업스트림을 보냅니다. 

## <a name="prerequisites"></a>필수 조건

* 빠른 시작 또는 이전 자습서에서 만든 Azure IoT Edge 장치
* IoT Edge 장치를 연결할 IoT Hub용 IoT Hub 연결 문자열  
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Visual Studio Code용 Azure IoT Edge 확장](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) (Visual Studio Code의 확장 패널에서 확장을 설치할 수 있음)
* [C# for Visual Studio Code(OmniSharp 제공) 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (Visual Studio Code의 확장 패널에서 확장을 설치할 수 있음)
* Visual Studio Code에 대한 Azure IoT Edge 확장
* [Docker](https://docs.docker.com/engine/installation/). 이 자습서에서는 플랫폼용 CE(Community Edition)로 충분합니다. VS Code를 실행하는 컴퓨터에 이를 설치해야 합니다.
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 

## <a name="choose-or-sign-up-for-a-docker-registry"></a>Docker 레지스트리 선택 또는 등록
이 자습서에서는 VS Code용 Azure IoT Edge 확장을 사용하여 모듈을 빌드하고 [Docker 이미지](https://docs.docker.com/glossary/?term=image)를 만듭니다. 그런 다음 [Docker 레지스트리](https://docs.docker.com/glossary/?term=registry)에 호스트된 [Docker 리포지토리](https://docs.docker.com/glossary/?term=repository)에 이 Docker 이미지를 푸시합니다. 마지막으로, 레지스트리에서 [Docker 컨테이너](https://docs.docker.com/glossary/?term=container)로 패키지된 Docker 이미지를 IoT Edge 장치에 배포합니다.  

이 자습서에서는 Docker 호환 레지스트리를 사용할 수 있습니다. 클라우드에서 사용 가능한 두 개의 인기 있는 Docker 레지스트리 서비스는 **Azure Container Registry** 및 **Docker Hub**입니다.

- [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/)는 [유료 구독](https://azure.microsoft.com/en-us/pricing/details/container-registry/)에서 사용할 수 있습니다. 이 자습서에서는 **기본** 구독으로 충분합니다. 

- Docker ID를 등록할 경우(무료) [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)에서는 하나의 무료 개인 리포지토리를 제공합니다. 
    1. Docker ID를 등록하려면 Docker 사이트에 나와 있는 [Docker ID 등록](https://docs.docker.com/docker-id/#register-for-a-docker-id)의 지침을 따릅니다. 

    2. 개인 Docker 리포지토리를 만들려면 Docker 사이트에 나와 있는 [Docker Hub에 새 리포지토리 만들기](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub)의 지침을 따릅니다.

이 자습서 전체에서 해당하는 경우, Azure Container Registry와 Docker Hub 모두에 대한 명령을 제공합니다.

## <a name="create-an-iot-edge-module-project"></a>IoT Edge 모듈 프로젝트 만들기
다음 단계는 Visual Studio Code 및 Azure IoT Edge 확장을 사용하여 .NET Core 2.0을 기반으로 IoT Edge 모듈을 만드는 방법을 보여 줍니다.
1. VS Code를 엽니다.
2. **보기 | 통합 터미널** 메뉴 명령을 사용하여 VS Code 통합 터미널을 엽니다.
3. 통합 터미널에서 다음 명령을 입력하여 dotnet에 **AzureIoTEdgeModule** 템플릿을 설치(또는 갱신)합니다.

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. 통합 터미널에서 다음 명령을 입력하여 새 모듈의 프로젝트를 만듭니다.

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```

    >[!NOTE]
    > 이 명령은 현재 작업 폴더에 프로젝트 폴더 **FilterModule**을 만듭니다. 다른 위치에 만들려면 명령을 실행하기 전에 디렉터리를 변경하세요.
 
3. **파일 | 폴더 열기** 메뉴 명령을 사용하여 **FilterModule** 폴더로 이동한 다음 **폴더 선택**을 클릭하여 VS Code에서 프로젝트를 엽니다.
4. VS Code 탐색기에서 **Program.cs**를 클릭하여 엽니다.
5. **Program** 클래스에 다음 필드를 추가합니다.

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

1. **Program** 클래스에 다음 클래스를 추가합니다. 이러한 클래스는 수신 메시지 본문의 예상 스키마를 정의합니다.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
    ```

1. **Init** 메서드에서 코드는 **DeviceClient** 개체를 만들고 구성합니다. 이 개체를 사용하면 메시지를 주고받기 위해 로컬 Azure IoT Edge 런타임에 모듈을 연결할 수 있습니다. **Init** 메서드에 사용되는 연결 문자열 매개 변수는 환경 변수 **EdgeHubConnectionString**에서 IoT Edge 런타임에 의해 모듈에 제공됩니다. **DeviceClient**를 만든 후 코드는 **input1** 끝점을 통해 IoT Edge 허브로부터 메시지를 수신하기 위한 콜백을 등록합니다. 메시지 처리를 위한 콜백으로 사용된 메서드를 새것으로 바꾸고, 모듈 트윈에서 원하는 속성을 업데이트하기 위한 콜백을 첨부하세요. 이 변경을 수행하려면 **Init** 메서드의 마지막 줄을 다음 코드로 바꾸세요.

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

1. 모듈 트윈을 통해 백 엔드 서비스에서 보낸 원하는 특성을 기반으로 **temperatureThreshold** 필드를 업데이트하려면 **Program** 클래스에 다음 메서드를 추가합니다. 모든 모듈에는 고유한 모듈 트윈이 있습니다. 모듈 트윈을 사용하면 백 엔드 서비스가 모듈 내부에서 실행 중인 코드를 구성할 수 있습니다.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"].exists())
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

1. **PipeMessage** 메서드를 다음 메서드로 바꿉니다. 이 메서드는 모듈이 Edge Hub에서 메시지를 보낼 때마다 호출되어, 메시지 본문의 온도 값 및 모듈 트윈을 통해 설정된 온도 임계값을 기반으로 메시지를 필터링합니다.

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        int counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            byte[] messageBytes = message.GetBytes();
            string messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get message body
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await deviceClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. 프로젝트를 빌드합니다. **보기 | 탐색** 메뉴 명령을 사용하여 VS Code 탐색기를 엽니다. 탐색기에서 **FilterModule.csproj** 파일을 마우스 오른쪽 단추로 클릭하고 **Build IoT Edge module**(IoT Edge 모듈 빌드)을 클릭하여 모듈을 컴파일한 다음, 이진 및 종속성을 다음 단계에서 Docker 이미지가 생성되는 폴더로 내보냅니다.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker 이미지를 만들어 레지스트리에 게시

1. Docker 이미지를 빌드합니다.
    1. VS Code 탐색기에서 **Docker** 폴더를 클릭하여 엽니다. 그런 다음 컨테이너 플랫폼의 폴더를 선택합니다(**linux-x64** 또는 **windows-nano**). 
    2. **Dockerfile** 파일을 마우스 오른쪽 단추로 클릭하고 **Build IoT Edge module Docker image**(Build IoT Edge 모듈 Docker 이미지)를 클릭합니다. 
    3. **폴더 선택** 상자에서 `./bin/Debug/netcoreapp2.0/publish`로 이동(또는 입력)합니다. **Select Folder as EXE_DIR**(EXE_DIR로 폴더 선택)을 클릭합니다.
    4. VS Code 창의 맨 위에 있는 팝업 텍스트 상자에 이미지 이름을 입력합니다. 예를 들어 `<docker registry address>/filtermodule:latest`에서 *docker registry address*는 Docker Hub를 사용하는 경우 Docker ID이고, Azure Container Registry를 사용하는 경우 `<your registry name>.azurecr.io`와 유사합니다.
 
4. Docker에 로그인합니다. 통합 터미널에서 다음 명령을 입력합니다. 

    - Docker Hub(메시지가 표시되면 자격 증명 입력):
        
        ```csh/sh
        docker login
        ```

    - Azure Container Registry의 경우:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        이 명령에서 사용할 사용자 이름, 암호 및 로그인 서버를 찾으려면 [Azure Portal](https://portal.azure.com)로 이동합니다. **모든 리소스**에서 Azure Container Registry에 대한 타일을 클릭하여 속성을 연 다음 **액세스 키**를 클릭합니다. **사용자 이름**, **암호** 및 **로그인 서버** 필드의 값을 복사합니다. 로그인 서버는 `<your registry name>.azurecr.io` 형식이어야 합니다.

3. Docker 리포지토리에 이미지를 푸시합니다. **보기 | 명령 팔레트... | Edge: Push IoT Edge module Docker image**(에지: IoT Edge 모듈 Docker 이미지 푸시) 메뉴 명령을 사용하여 VS Code 창의 맨 위에 있는 팝업 텍스트 상자에 이미지 이름을 입력합니다. 1.c단계에서 사용한 동일한 이미지 이름을 사용하세요.

## <a name="add-registry-credentials-to-edge-runtime-on-your-edge-device"></a>에지 장치의 에지 런타임에 레지스트리 자격 증명 추가
에지 장치를 실행 중인 컴퓨터의 에지 런타임에 레지스트리의 자격 증명을 추가합니다. 이는 컨테이너를 끌어오기 위한 런타임 액세스 권한을 제공합니다. 

- Windows의 경우 다음 명령을 실행합니다.
    
    ```cmd/sh
    iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

- Linux의 경우 다음 명령을 실행합니다.
    
    ```cmd/sh
    sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

## <a name="run-the-solution"></a>솔루션 실행

1. [Azure Portal](https://portal.azure.com)에서 IoT Hub로 이동합니다.
2. **IoT Edge(미리 보기)**로 이동하여 IoT Edge 장치를 선택합니다.
3. **모듈 설정**을 선택합니다. 
2. **tempSensor** 모듈을 추가합니다. 이 단계는 이전에 **tempSensor** 모듈을 IoT Edge 장치에 배포하지 않은 경우에만 필요합니다.
    1. **Add IoT Edge Module**(IoT Edge 모듈 추가)을 선택합니다.
    2. **이름** 필드에 `tempSensor`를 입력합니다.
    3. **이미지 URI** 필드에 `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`를 입력합니다.
    4. 다른 설정은 변경하지 않고 **저장**을 클릭합니다.
9. **filtermodule**을 추가합니다.
    1. **Add IoT Edge Module**(IoT Edge 모듈 추가)을 다시 선택합니다.
    2. **이름** 필드에 `filtermodule`을 입력합니다.
    3. **이미지** 필드에 이미지 주소(예: `<docker registry address>/filtermodule:latest`)를 입력합니다.
    4. **Edit module twin**(모듈 트윈 편집) 확인란을 선택합니다.
    5. 모듈 트윈의 텍스트 상자에 있는 JSON을 다음 JSON으로 바꿉니다. 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. **Save**를 클릭합니다.
12. **다음**을 누릅니다.
13. **경로 지정** 단계에서 다음 JSON을 텍스트 상자에 복사합니다. 모듈은 모든 메시지를 Edge 런타임에 게시합니다. 런타임의 선언적 규칙은 해당 메시지가 어디로 흐르는지를 정의합니다. 이 자습서에서는 두 개의 경로가 필요합니다. 첫 번째 경로는 **FilterMessages** 핸들러로 구성한 엔드포인트인 “input1” 엔드포인트를 통해 온도 센서에서 필터 모듈로 메시지를 전송합니다. 두 번째 경로는 필터 모듈에서 IoT Hub로 메시지를 전송합니다. 이 경로에서 `upstream`은 메시지를 IoT Hub로 보내라고 Edge Hub에 알리는 특수 대상입니다. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. **다음**을 누릅니다.
5. **템플릿 검토** 단계에서 **제출**을 클릭합니다. 
6. IoT Edge 장치 세부 정보 페이지로 돌아가서 **새로 고침**을 클릭합니다. **tempSensor** 모듈 및 **IoT Edge runtime**과 함께 실행되는 새로운 **filtermodule**이 표시됩니다. 

## <a name="view-generated-data"></a>생성된 데이터 보기

IoT Edge 장치에서 IoT Hub로 보낸 장치-클라우드 메시지를 모니터하려면 다음을 수행하세요.
1. IoT Hub용 연결 문자열로 Azure IoT Toolkit 확장을 구성합니다. 
    1. **보기 | 탐색** 메뉴 명령을 사용하여 VS Code 탐색기를 엽니다. 
    3. 탐색기에서 **IOT HUB 장치**를 클릭하고 **...**을 클릭합니다. **Set IoT Hub Connection String**(IoT Hub 연결 문자열 설정)을 클릭하고, 팝업 창에서 IoT Edge 장치를 연결할 IoT Hub용 연결 문자열을 입력합니다. 

        연결 문자열을 찾으려면 Azure Portal에서 IoT Hub의 타일을 클릭한 다음 **공유 액세스 정책**을 클릭합니다. **공유 액세스 정책**에서 **iothubowner** 정책을 클릭한 다음 **iothubowner** 창에 있는 IoT Hub 연결 문자열을 복사합니다.   

1. IoT Hub에 도착하는 데이터를 모니터링하려면 **보기 | 명령 팔레트... | IoT: Start monitoring D2C message**(D2C 메시지 모니터링 시작) 메뉴 명령을 사용합니다. 
2. 데이터 모니터링을 중지하려면 **보기 | 명령 팔레트... | IoT: Stop monitoring D2C message**(D2C 메시지 모니터링 중지) 메뉴 명령을 사용합니다. 

## <a name="next-steps"></a>다음 단계

이 자습서에서는 IoT Edge 장치에서 생성된 원시 데이터를 필터링하는 코드가 포함된 IoT Edge 모듈을 만들었습니다. Azure IoT Edge에서 데이터를 통해 비즈니스 통찰력을 얻는 데 도움이 되는 다른 방법을 알아보려면 다음 자습서 중 하나를 계속 진행할 수 있습니다.

> [!div class="nextstepaction"]
> [Azure Functions를 모듈로 배포](tutorial-deploy-function.md)
> [Azure Stream Analytics를 모듈로 배포](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
