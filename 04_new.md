# 練習 4：開始使用 Azure IoT Edge

## 案例

由於部署的裝置數量龐大，因此也會收集到大量資料並傳送至雲端。能夠將這些情報送到 Edge 嗎？

Fabrikam, Inc. 想要使用 IoT Edge 閘道裝置，將一些情報送到 Edge 立即進行處理。一部分的資料仍然會傳送到雲端。將資料情報送到 IoT Edge，也可確保即使在區域網路連線不佳的情況下，仍能夠處理資料並迅速做出回應。

為此，要求您為 Azure IoT Edge 解決方案進行原型設計的工作。一開始，您會將串流分析模組部署至要用來計算平均溫度的裝置，並在超過處理控制項的值時產生警示通知。

## 概觀

在此練習中，您會將串流分析模組部署至 Edge 裝置，並在超過處理控制項的值時產生警示通知。基於此實驗室的目的，為您提供的預先建立 Linux Azure VM 已設定為 IoT Edge 裝置。

您會在此練習當中完成下列各項工作：

* 連線至 IoT Edge VM
* 對 Edge 裝置新增 Edge 模組
* 對 Edge 裝置部署 Azure 串流分析 Edge 模組

Azure IoT Edge 是由在雲端中執行的雲端服務，以及在裝置上執行的執行階段組合而成。執行階段在裝置上啟動和管理工作流程。工作流程由一組容器所組成，您能以特定順序將容器連結在一起，建立端對端案例。IoT Edge 是由 IoT 中樞所管理的。Azure IoT Edge 可讓您在使用於雲端服務開發的 Edge 裝置上執行工作負載。工作負載是使用 Docker 相容的容器所部署的模組。人工智慧應用程式、Azure 及協力廠商服務，或您的商務邏輯，都可以是模組。如需有關 IoT Edge 的詳細資訊，請瀏覽此連結：```https://docs.microsoft.com/en-us/azure/iot-edge/about-iot-edge```

## 解決方案架構
 
  ![實驗室 04 架構](media/lab4_architecture.png)

### 工作 1：連線至 IoT Edge VM

在此工作中，您會連線至 IoT Edge VM，並驗證裝置上是否正在執行 Azure IoT Edge。

1. 在 Azure 入口網站功能表上，按一下 **[資源群組]**。
 
1. 按一下 **iot-{deployment-id}** 資源群組中的 IoT Edge 虛擬機器 **linuxagentvm-{deployment-id}**。

1. 在 **[概觀]** 窗格頂端，按一下 **[連線]**，然後按一下 **[SSH]**。

1. 在 **[連線]** 窗格上，於 **[4.執行下面的範例命令以連線至 VM]** 下方，複製範例命令。

    這是範例 SSH 命令，可用來連線至虛擬機器，該虛擬機器中包含 VM 的 IP 位址和系統管理員使用者名稱。命令的格式應該類似於 `ssh demouser@52.170.205.79`。

    > **注意**：您複製的範例命令包含 **-i <私密金鑰路徑>**，請使用文字編輯器來移除命令的這一部分，然後將更新的命令複製到剪貼簿。
 
1. 瀏覽到 **```https://shell.azure.com```** 來開啟 Azure Cloud Shell，然後選取 **[Bash]**。

1. 按一下 **[顯示進階設定]**，然後提供下列詳細資料：

   * 資源群組：選取 **[使用現有項目]** -> **iot-{deployment-id}**
   * 儲存體帳戶：選取 **[建立新項目]**，然後輸入 **cloudstore{deployment-id}**
   * 檔案共用：選取 **[建立新項目]**，然後輸入 **blob**
   
     >   **注意**：- 您可以從環境詳細資料頁面中取得 **deployment-id** 詳細資料。
        
   ![Azure 入口網站螢幕擷取畫面顯示要建立儲存體帳戶的選取路徑。](media/storageaccount01.png)

1. 在 Azure Cloud Shell 命令提示字元中，貼上您在文字編輯器中更新的 `ssh` 命令，然後按 **Enter**。

1. 看到 **[Are you sure you want to continue connecting? (您要繼續連線嗎?)]** 的提示時，輸入 `yes`，然後按 **Enter**。

1. 提示您輸入密碼時，輸入 **Password.1!!**，然後按 **Enter**。

   ![連線至 linuxagent 虛擬機器。](media/vmlogin.png)

1. 連線之後，終端機命令提示字元會變成顯示 Linux VM 的名稱，看起來和下面類似。

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$
    ```

    這會告訴您與哪一部 VM 連線。

    > **重要：**連線時，很可能會告訴您，Edge VM 有許多待安裝的 OS 更新。  基於實驗室的目的，我們可以將此忽略，但在生產環境中，請務必確定 Edge 裝置總是維持在最新狀態。

1. 若要確認已在 VM 上安裝 Azure IoT Edge 執行階段，請執行下列命令：

    ```cmd/sh
    iotedge version
    ```

    此命令會輸出目前安裝在虛擬機器上的 Azure IoT Edge 執行階段版本。
    IoT Edge 裝置上都會安裝 IoT Edge 執行階段。IoT Edge 執行階段是可將裝置轉變成 IoT Edge 裝置的程式集合。總而言之，IoT Edge 執行階段元件可讓 IoT Edge 裝置接收程式碼以在邊緣執行，並將結果傳達給 IoT 中樞。

### 工作 2：對 Edge 裝置新增 Edge 模組

在此練習中，您會將模擬的溫度感應器新增為自訂的 IoT Edge 模組，然後加以部署以在 IoT Edge 裝置上執行。

IoT Edge 模組是實作為容器的可執行套件。

透過 IoT Edge 模組，您可以部署雲端工作負載，直接在 IoT 裝置上執行。IoT Edge 模組是由 IoT Edge 所管理的最小計算單位。使用 IoT Edge 模組，您可以分析裝置上的資料，而不是雲端上的資料。將部分工作負載移至 Edge，可讓裝置花費較少的時間將訊息傳送至雲端，並能更快速對事件做出反應。

1. 如有必要，請使用您的 Azure 帳戶認證來登入 Azure 入口網站。
 
1. 在您的資源群組磚上，若要開啟 IoT Hub，請按一下 **iothub-{deployment-id}**。

1. 在 **[IoT 中樞]** 刀鋒視窗左側的 **[自動裝置管理]** 下，按一下 **[IoT Edge]**。

1. 在 IoT Edge 裝置清單上，按一下 **turbine-06**。

1. 請注意，**turbine-06** 刀鋒視窗上的 **[模組]** 索引標籤會顯示目前為裝置所設定模組的清單。

    目前，將 IoT Edge 裝置設定為只有 Edge 代理程式 (`$edgeAgent`) 與 Edge 中樞 (`$edgeHub`) 模組，其均為 IoT Edge 執行階段的一部分。

1. 在 **turbine-06** 刀鋒視窗頂端，按一下 **[設定模組]**。

1. 在 **[在裝置上設定模組: turbine-06]** 刀鋒視窗上，找到 **[IoT Edge 模組]** 區段。

1. 在 **[IoT Edge 模組]** 下，按一下 **[新增]**，然後按一下 **[IoT Edge 模組]**。

1. 在 **[新增 IoT Edge 模組]** 窗格的 **[IoT Edge 模組名稱]** 下，輸入 **turbinesensor**

    我們會將自訂模組命名為 "turbinesensor"

1. 在 **[映像 URI]** 下，輸入 **asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor**

    > **注意**：此映像是在產品小組所提供 Docker Hub 上發佈的映像，可以支援此測試案例。

1. 若要變更選取的索引標籤，請按一下 **[模組對應項設定]**。

1. 若要指定模組對應項所需的屬性，請輸入下列 JSON：

    ```json
    {
        "EnableProtobufSerializer": false,
        "EventGeneratingSettings": {
            "IntervalMilliSec": 500,
            "PercentageChange": 2,
            "SpikeFactor": 2,
            "StartValue": 68,
            "SpikeFrequency": 20
        }
    }
    ```

    此 JSON 透過設定其模組對應項所需的屬性，來設定 Edge 模組。

1. 在刀鋒視窗底部，按一下 **[新增]**。

1. 在 **[在裝置上設定模組: turbine-06]** 刀鋒視窗的底部，按一下 **[下一步: 路由 >]**。

1. 請注意，已經設定預設路徑。

    * 名稱：**route**
    * 值：`FROM /messages/* INTO $upstream`

    此路由會將 IoT Edge 裝置上所有模組的全部訊息都傳送至 IoT 中樞

1. 按一下 **[檢閱 + 建立]**。

1. 在 **[部署]** 下，請花一分鐘的時間，檢閱顯示的部署資訊清單。 

    如您所見，IoT Edge 裝置的部署資訊清單會格式化為 JSON，因此相當容易閱讀。

    `properties.desired` 區段的下方是 `modules` 區段，此區段宣告要部署至 IoT Edge 裝置的 IoT Edge 模組。這包括所有模組的映像 URI，包括任何容器登錄認證在內。

    ```json
    {
        "modulesContent": {
            "$edgeAgent": {
                "properties.desired": {
                    "modules": {
                        "turbinesensor": {
                            "settings": {
                                "image": "asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor",
                                "createOptions": ""
                            },
                            "type": "docker",
                            "version": "1.0",
                            "status": "running",
                            "restartPolicy": "always"
                        },
    ```

    JSON 的下半部是 **$edgeHub** 區段，其中包含 Edge 中樞所需的屬性。此區段也包含可供在模組之間路由事件，以及路由至 IoT 中樞的路由組態。

    ```json
        "$edgeHub": {
            "properties.desired": {
                "routes": {
                  "route": "FROM /messages/* INTO $upstream"
                },
                "schemaVersion": "1.0",
                "storeAndForwardConfiguration": {
                    "timeToLiveSecs": 7200
                }
            }
        },
    ```

    JSON 的下半部還有 **turbinesensor** 模組的區段，其中的 `properties.desired` 區段包含 Edge 模組的組態所需的屬性。

    ```json
                },
                "turbinesensor": {
                    "properties.desired": {
                        "EnableProtobufSerializer": false,
                        "EventGeneratingSettings": {
                            "IntervalMilliSec": 500,
                            "PercentageChange": 2,
                            "SpikeFactor": 2,
                            "StartValue": 68,
                            "SpikeFrequency": 20
                        }
                    }
                }
            }
        }
    ```

1. 若要完成設定裝置的模組，請在刀鋒視窗底部，按一下 **[建立]**。

1. 請注意，在 **turbine-06** 刀鋒視窗的 **[模組]** 下，現在已列出 **turbinesensor**。

    > **注意**：您可能必須按一下 **[重新整理]**，才能看到第一次列出的模組。

    您可能會注意到並未回報 **turbinesensor** 的 [執行階段狀態]。

1. 在刀鋒視窗頂端，按一下 **[重新整理]**。

1. 請注意，**turbinesensor** 模組的 **[執行階段狀態]** 現在設定為 **[執行中]**。

    如果仍未回報值，請稍候片刻，然後再次重新整理刀鋒視窗。
 
1. 開啟 Cloud Shell 工作階段 (如果尚未開啟的話)。

    如果您不再連線至 `vm-iot-edge-{deployment-id}` 虛擬機器，請如往常一樣使用 **SSH** 來連線。

1. 若要列出目前在 IoT Edge 裝置上執行的模組，請在 Cloud Shell 命令提示字元輸入下列命令：

    ```cmd/sh
    iotedge list
    ```

1. 命令的輸出看起來和下面類似。 

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME             STATUS           DESCRIPTION      CONFIG
    edgeHub          running          Up a minute      mcr.microsoft.com/azureiotedge-hub:1.0
    edgeAgent        running          Up 26 minutes    mcr.microsoft.com/azureiotedge-agent:1.0
    turbinesensor    running          Up 34 seconds    asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ```

    請注意，`turbinesensor` 已列為其中一個執行中的模組。

1. 若要檢視模組記錄，請輸入下列命令：

    ```cmd/sh
    iotedge logs turbinesensor
    ```

    命令的輸出看起來和下面類似：

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge logs turbinesensor
    11/14/2019 18:05:02 - Send Json Event : {"machine":{"temperature":41.199999999999925,"pressure":1.0182182583425192},"ambient":{"temperature":21.460937846433808,"humidity":25},"timeCreated":"2019-11-14T18:05:02.8765526Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.599999999999923,"pressure":1.0185790159334602},"ambient":{"temperature":20.51992724976499,"humidity":26},"timeCreated":"2019-11-14T18:05:03.3789786Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.999999999999922,"pressure":1.0189397735244012},"ambient":{"temperature":20.715225311096397,"humidity":26},"timeCreated":"2019-11-14T18:05:03.8811372Z"}
    ```

    `iotedge logs` 命令可以用來檢視任何 Edge 模組的模組記錄。

1. 模擬溫度感應器模組會在傳送 500 則訊息之後停止。執行下列命令即可重新啟動：

    ```cmd/sh
    iotedge restart turbinesensor
    ```

    您不需要立即重新啟動模組，但如果您稍候發現模組停止傳送遙測，請返回 Cloud Shell，SSH 至 Edge VM，然後執行此命令予以重設。重設之後，模組會再次開始傳送遙測。

### 工作 3：部署 Azure 串流分析作為 IoT Edge 模組

現在，**turbinesensor** 模組已部署在 IoT Edge 裝置上並處於正在執行，我們可以新增串流分析模組，如此可先在 IoT Edge 裝置本身處理訊息，然後再傳送到 IoT 中樞。

#### 檢閱串流分析作業

1. 在資源群組磚上，按一下 **iot-{deployment-id}**，然後選取串流分析作業名稱 **iotedge-streamjob-{deployment-id}**。

1. 然後在刀鋒視窗左側的 **[作業拓撲]** 下，選取 **[輸入]**，然後確認已定義一個**溫度**輸入工作。

1. 接著，選取 **[作業拓撲]** 下的 **[輸出]**，然後確認已經定義**警示**輸出工作。

1. 在左側導覽功能表的 **[設定]** 下，按一下 [儲存體帳戶設定]。然後，確定已經新增 iotstorage{deployment-id} 儲存體帳戶。

#### 部署串流分析作業

1. 在 Azure 入口網站中，瀏覽到 **iothub-{deployment-id}** IoT 中樞資源。

1. 在左側導覽功能表的 **[自動裝置管理]** 下，按一下 **[IoT Edge]**。

1. 在 **[裝置識別碼]** 下，按一下 **turbine-06**。

1. 在 **turbine-06** 窗格頂端，按一下 **[設定模組]**。

1. 在 **[在裝置上設定模組: turbine-06]** 窗格上，找到 **[IoT Edge 模組]** 區段。

1. 在 **[IoT Edge 模組]** 下，按一下 **[新增]**，然後按一下 **[Azure 串流分析模組]**。

1. 在 **[Edge 部署]** 窗格的 **[訂用帳戶]** 下，確定已選取於此課程使用的訂用帳戶。

1. 在 **[Edge 工作]** 下拉式清單中，確定已選取 **iotedge-streamjob-{deployment-id}** 串流分析作業。

    > **注意**：您可能已經選取該工作，但 **[儲存]** 按鈕處於停用狀態 - 只要再次開啟 **[Edge 工作]** 下拉式清單，並再次選取 **iotedge-streamjob-{deployment-id}** 工作。應會啟用 **[儲存]** 按鈕。

1. 在窗格底部，按一下 **[儲存]**。

    部署可能需要花費一些時間。

1. 在 **[在裝置上設定模組: turbine-06]** 刀鋒視窗底部，按一下 **[檢閱 + 建立]**。

1. 在 **[檢閱 + 建立]** 索引標籤上，請注意**部署資訊清單** JSON 現在會更新為串流分析模組和剛設定的路由定義。

1. 在刀鋒視窗底部，按一下 **[建立]**。

1. 成功發佈 Edge 套件之後，請注意新的 ASA 模組就會列在 **[IoT Edge 模組]** 區段下方。

1. 在 **[IoT Edge 模組]** 下，按一下 **iotedge-streamjob-{deployment-id}**。

    這是剛才新增至 Edge 裝置的串流分析模組。

1. 請注意，**[更新 IoT Edge 模組]** 窗格上的 **[映像 URI]** 指向標準的 Azure 串流分析映像。

    ```text
    mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.7
    ```

    這是部署至 IoT Edge 裝置的每項 ASA 作業，都會使用的相同映像。

    > **注意**：  當您建立串流分析模組時，**[映像 URI]** 結尾的版本號碼設定為反映目前的最新版本。

1. 讓所有值都維持其預設值，然後關閉 **[IoT Edge 自訂模組]** 窗格。

1. 在 **[在裝置上設定模組: turbine-06]** 刀鋒視窗上，按一下 **[下一步: 路由 >]**。

    請注意，現有路由隨即顯示。

1. 請以下列三個路由取代預設所定義的路由：
   
   > **注意**：請務必以 Azure 串流分析作業模組的名稱來取代 `iotstreamjob-edge-{deployment-id}` 預留位置。
   
    * 路由 1
        * 名稱：**telemetryToCloud**
        * 值：`FROM /messages/modules/turbinesensor/* INTO $upstream`
    * 路由 2
        * 名稱：**alertsToReset**
        * 值：`FROM /messages/modules/iotedge-streamjob-{deployment-id}/* INTO BrokeredEndpoint("/modules/turbinesensor/inputs/control")`
    * 路由 3
        * 名稱：**telemetryToAsa**
        * 值：`FROM /messages/modules/turbinesensor/* INTO BrokeredEndpoint("/modules/iotedge-streamjob-{deployment-id}/inputs/temperature")`

    > **注意**：您可以按一下 **[上一步]**，檢視模組清單與各模組的名稱，然後按 **[下一步]**，返回此步驟。

    所定義的路由如下所示：

    * **telemetryToCloud** 路由會將來自 `turbinesensor` 模組輸出的所有訊息傳送至 Azure IoT 中樞。
    * **alertsToReset** 路由會將來自串流分析模組輸出的所有警示訊息傳送至 **turbinesensor** 模組的輸入。
    * **telemetryToAsa** 路由會將來自 `turbinesensor` 模組輸出的所有訊息傳送至串流分析模組輸入。

1. 在 **[在裝置上設定模組: turbine-06]** 刀鋒視窗底部，按一下 **[檢閱 + 建立]**。

1. 在 **[檢閱 + 建立]** 索引標籤上，請注意**部署資訊清單** JSON 現在會更新為串流分析模組和剛設定的路由定義。

1. 請注意，`turbinesensor` 模擬溫度感應器模組的 JSON 組態：

    ```json
    "turbinesensor": {
        "settings": {
            "image": "asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor",
            "createOptions": ""
        },
        "type": "docker",
        "version": "1.0",
        "status": "running",
        "restartPolicy": "always"
    },
    ```

1. 請注意，先前所設定路由的 JSON 組態，以及如何以 JSON 部署定義來設定：

    ```json
    "$edgeHub": {
        "properties.desired": {
            "routes": {
                "telemetryToCloud": "FROM /messages/modules/turbinesensor/* INTO $upstream",
                "alertsToReset": "FROM /messages/modules/iotedge-streamjob-{deployment-id}/* INTO BrokeredEndpoint(\\\"/modules/turbinesensor/inputs/control\\\")",
                "telemetryToAsa": "FROM /messages/modules/turbinesensor/* INTO BrokeredEndpoint(\\\"/modules/iotedge-streamjob-{deployment-id}/inputs/temperature\\\")"
            },
            "schemaVersion": "1.0",
            "storeAndForwardConfiguration": {
                "timeToLiveSecs": 7200
            }
        }
    },
    ```

1. 在刀鋒視窗底部，按一下 **[建立]**。

#### 檢視資料

1. 返回 **Cloud Shell** 工作階段，您在此處透過 **SSH** 連線至 **IoT Edge 裝置**。  

    > **注意**：如果連線已關閉或逾時，請重新連線。執行 `SSH` 命令，並和往常一樣地登入。

1. 若要檢視部署至裝置的模組清單，請在命令提示字元輸入下列命令：

    ```cmd/sh
    iotedge list
    ```

    需要花一點時間完成將新的串流分析模組部署至 IoT Edge 裝置。完成之後，即可透過此命令在清單輸出中看到該模組。

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME                       STATUS           DESCRIPTION      CONFIG
    iotedge-streamjob-232539   running          Up a minute      mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5
    edgeAgent                  running          Up 6 hours       mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                    running          Up 4 hours       mcr.microsoft.com/azureiotedge-hub:1.0
    turbinesensor              running          Up 4 hours       asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ``` 

    > **注意**：  如果清單中未顯示串流分析模組，請靜候一至二分鐘，然後再試一次。需要花一點時間完成在 IoT Edge 裝置上更新模組部署。

1. 若要觀看由 `turbinesensor` 模組從 Edge 裝置傳送的遙測，請在命令提示字元輸入下列命令：

    ```cmd/sh
    iotedge logs turbinesensor
    ```

1. 請花一分鐘的時間，觀察一下輸出。
 
    此事件的輸出看起來和下面類似：

    ```cmd/sh
    11/14/2019 22:26:44 - Send Json Event : {"machine":{"temperature":231.599999999999959,"pressure":1.0095600761599359},"ambient":{"temperature":21.430643635304012,"humidity":24},"timeCreated":"2019-11-14T22:26:44.7904425Z"}
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":531.999999999999957,"pressure":1.0099208337508767},"ambient":{"temperature":20.569532965342297,"humidity":25},"timeCreated":"2019-11-14T22:26:45.2901801Z"}
    Received message
    Received message Body: [{"command":"reset"}]
    Received message MetaData: {"MessageId":null,"To":null,"ExpiryTimeUtc":"0001-01-01T00:00:00","CorrelationId":null,"SequenceNumber":0,"LockToken":"e0e778b5-60ff-4e5d-93a4-ba5295b995941","EnqueuedTimeUtc":"0001-01-01T00:00:00","DeliveryCount":0,"UserId":null,"MessageSchema":null,"CreationTimeUtc":"0001-01-01T00:00:00","ContentType":"application/json","InputName":"control","ConnectionDeviceId":"turbine-06","ConnectionModuleId":"vm-iot-edge-CP1119","ContentEncoding":"utf-8","Properties":{},"BodyStream":{"CanRead":true,"CanSeek":false,"CanWrite":false,"CanTimeout":false}}
    Resetting temperature sensor..
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":320.4,"pressure":0.99945886361358849},"ambient":{"temperature":20.940019742324957,"humidity":26},"timeCreated":"2019-11-14T22:26:45.7931201Z"}
    ```
 
1. 請注意，在觀看由 **turbinesensor** 傳送的溫度遙測時，當 `machine.temperature` 的平均超過 `72` 時，串流分析作業就會傳送 **reset** 命令。這就是在串流分析作業查詢中設定的動作。

您已在此練習中利用 Azure IoT Edge 服務，在 Edge 裝置上處理訊息。
