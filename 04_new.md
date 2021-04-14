# �m�� 4�G�}�l�ϥ� Azure IoT Edge

## �ר�

�ѩ󳡸p���˸m�ƶq�e�j�A�]���]�|������j�q��ƨöǰe�ܶ��ݡC����N�o�Ǳ����e�� Edge �ܡH

Fabrikam, Inc. �Q�n�ϥ� IoT Edge �h�D�˸m�A�N�@�Ǳ����e�� Edge �ߧY�i��B�z�C�@��������Ƥ��M�|�ǰe�춳�ݡC�N��Ʊ����e�� IoT Edge�A�]�i�T�O�Y�Ϧb�ϰ�����s�u���Ϊ����p�U�A������B�z��ƨè��t���X�^���C

�����A�n�D�z�� Azure IoT Edge �ѨM��׶i��쫬�]�p���u�@�C�@�}�l�A�z�|�N��y���R�Ҳճ��p�ܭn�Ψӭp�⥭���ūת��˸m�A�æb�W�L�B�z������Ȯɲ���ĵ�ܳq���C

## ���[

�b���m�ߤ��A�z�|�N��y���R�Ҳճ��p�� Edge �˸m�A�æb�W�L�B�z������Ȯɲ���ĵ�ܳq���C��󦹹���Ǫ��ت��A���z���Ѫ��w���إ� Linux Azure VM �w�]�w�� IoT Edge �˸m�C

�z�|�b���m�߷������U�C�U���u�@�G

* �s�u�� IoT Edge VM
* �� Edge �˸m�s�W Edge �Ҳ�
* �� Edge �˸m���p Azure ��y���R Edge �Ҳ�

Azure IoT Edge �O�Ѧb���ݤ����檺���ݪA�ȡA�H�Φb�˸m�W���檺���涥�q�զX�Ӧ��C���涥�q�b�˸m�W�ҰʩM�޲z�u�@�y�{�C�u�@�y�{�Ѥ@�ծe���Ҳզ��A�z��H�S�w���ǱN�e���s���b�@�_�A�إߺݹ�ݮרҡCIoT Edge �O�� IoT ���ϩҺ޲z���CAzure IoT Edge �i���z�b�ϥΩ󶳺ݪA�ȶ}�o�� Edge �˸m�W����u�@�t���C�u�@�t���O�ϥ� Docker �ۮe���e���ҳ��p���ҲաC�H�u���z���ε{���BAzure �Ψ�O�t�ӪA�ȡA�αz���Ӱ��޿�A���i�H�O�ҲաC�p�ݦ��� IoT Edge ���ԲӸ�T�A���s�����s���G```https://docs.microsoft.com/en-us/azure/iot-edge/about-iot-edge```

## �ѨM��׬[�c
 
  ![����� 04 �[�c](media/lab4_architecture.png)

### �u�@ 1�G�s�u�� IoT Edge VM

�b���u�@���A�z�|�s�u�� IoT Edge VM�A�����Ҹ˸m�W�O�_���b���� Azure IoT Edge�C

1. �b Azure �J�f�����\���W�A���@�U **[�귽�s��]**�C
 
1. ���@�U **iot-{deployment-id}** �귽�s�դ��� IoT Edge �������� **linuxagentvm-{deployment-id}**�C

1. �b **[���[]** ���泻�ݡA���@�U **[�s�u]**�A�M����@�U **[SSH]**�C

1. �b **[�s�u]** ����W�A�� **[4.����U�����d�ҩR�O�H�s�u�� VM]** �U��A�ƻs�d�ҩR�O�C

    �o�O�d�� SSH �R�O�A�i�Ψӳs�u�ܵ��������A�ӵ����������]�t VM �� IP ��}�M�t�κ޲z���ϥΪ̦W�١C�R�O���榡���������� `ssh demouser@52.170.205.79`�C

    > **�`�N**�G�z�ƻs���d�ҩR�O�]�t **-i <�p�K���_���|>**�A�ШϥΤ�r�s�边�Ӳ����R�O���o�@�����A�M��N��s���R�O�ƻs��ŶKï�C
 
1. �s���� **```https://shell.azure.com```** �Ӷ}�� Azure Cloud Shell�A�M���� **[Bash]**�C

1. ���@�U **[��ܶi���]�w]**�A�M�ᴣ�ѤU�C�ԲӸ�ơG

   * �귽�s�աG��� **[�ϥβ{������]** -> **iot-{deployment-id}**
   * �x�s��b��G��� **[�إ߷s����]**�A�M���J **cloudstore{deployment-id}**
   * �ɮצ@�ΡG��� **[�إ߷s����]**�A�M���J **blob**
   
     >   **�`�N**�G- �z�i�H�q���ҸԲӸ�ƭ��������o **deployment-id** �ԲӸ�ơC
        
   ![Azure �J�f�����ù��^���e����ܭn�إ��x�s��b�᪺������|�C](media/storageaccount01.png)

1. �b Azure Cloud Shell �R�O���ܦr�����A�K�W�z�b��r�s�边����s�� `ssh` �R�O�A�M��� **Enter**�C

1. �ݨ� **[Are you sure you want to continue connecting? (�z�n�~��s�u��?)]** �����ܮɡA��J `yes`�A�M��� **Enter**�C

1. ���ܱz��J�K�X�ɡA��J **Password.1!!**�A�M��� **Enter**�C

   ![�s�u�� linuxagent ���������C](media/vmlogin.png)

1. �s�u����A�׺ݾ��R�O���ܦr���|�ܦ���� Linux VM ���W�١A�ݰ_�өM�U�������C

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$
    ```

    �o�|�i�D�z�P���@�� VM �s�u�C

    > **���n�G**�s�u�ɡA�ܥi��|�i�D�z�AEdge VM ���\�h�ݦw�˪� OS ��s�C  ������Ǫ��ت��A�ڭ̥i�H�N�������A���b�Ͳ����Ҥ��A�аȥ��T�w Edge �˸m�`�O�����b�̷s���A�C

1. �Y�n�T�{�w�b VM �W�w�� Azure IoT Edge ���涥�q�A�а���U�C�R�O�G

    ```cmd/sh
    iotedge version
    ```

    ���R�O�|��X�ثe�w�˦b���������W�� Azure IoT Edge ���涥�q�����C
    IoT Edge �˸m�W���|�w�� IoT Edge ���涥�q�CIoT Edge ���涥�q�O�i�N�˸m���ܦ� IoT Edge �˸m���{�����X�C�`�Ө����AIoT Edge ���涥�q����i�� IoT Edge �˸m�����{���X�H�b��t����A�ñN���G�ǹF�� IoT ���ϡC

### �u�@ 2�G�� Edge �˸m�s�W Edge �Ҳ�

�b���m�ߤ��A�z�|�N�������ū׷P�����s�W���ۭq�� IoT Edge �ҲաA�M��[�H���p�H�b IoT Edge �˸m�W����C

IoT Edge �ҲլO��@���e�����i����M��C

�z�L IoT Edge �ҲաA�z�i�H���p���ݤu�@�t���A�����b IoT �˸m�W����CIoT Edge �ҲլO�� IoT Edge �Һ޲z���̤p�p����C�ϥ� IoT Edge �ҲաA�z�i�H���R�˸m�W����ơA�Ӥ��O���ݤW����ơC�N�����u�@�t������ Edge�A�i���˸m��O���֪��ɶ��N�T���ǰe�ܶ��ݡA�ï��ֳt��ƥ󰵥X�����C

1. �p�����n�A�Шϥαz�� Azure �b��{�Ҩӵn�J Azure �J�f�����C
 
1. �b�z���귽�s�տj�W�A�Y�n�}�� IoT Hub�A�Ы��@�U **iothub-{deployment-id}**�C

1. �b **[IoT ����]** �M�W���������� **[�۰ʸ˸m�޲z]** �U�A���@�U **[IoT Edge]**�C

1. �b IoT Edge �˸m�M��W�A���@�U **turbine-06**�C

1. �Ъ`�N�A**turbine-06** �M�W�����W�� **[�Ҳ�]** ���޼��ҷ|��ܥثe���˸m�ҳ]�w�Ҳժ��M��C

    �ثe�A�N IoT Edge �˸m�]�w���u�� Edge �N�z�{�� (`$edgeAgent`) �P Edge ���� (`$edgeHub`) �ҲաA�䧡�� IoT Edge ���涥�q���@�����C

1. �b **turbine-06** �M�W�������ݡA���@�U **[�]�w�Ҳ�]**�C

1. �b **[�b�˸m�W�]�w�Ҳ�: turbine-06]** �M�W�����W�A��� **[IoT Edge �Ҳ�]** �Ϭq�C

1. �b **[IoT Edge �Ҳ�]** �U�A���@�U **[�s�W]**�A�M����@�U **[IoT Edge �Ҳ�]**�C

1. �b **[�s�W IoT Edge �Ҳ�]** ���檺 **[IoT Edge �ҲզW��]** �U�A��J **turbinesensor**

    �ڭ̷|�N�ۭq�ҲթR�W�� "turbinesensor"

1. �b **[�M�� URI]** �U�A��J **asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor**

    > **�`�N**�G���M���O�b���~�p�թҴ��� Docker Hub �W�o�G���M���A�i�H�䴩�����ծרҡC

1. �Y�n�ܧ��������޼��ҡA�Ы��@�U **[�Ҳչ������]�w]**�C

1. �Y�n���w�Ҳչ������һݪ��ݩʡA�п�J�U�C JSON�G

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

    �� JSON �z�L�]�w��Ҳչ������һݪ��ݩʡA�ӳ]�w Edge �ҲաC

1. �b�M�W���������A���@�U **[�s�W]**�C

1. �b **[�b�˸m�W�]�w�Ҳ�: turbine-06]** �M�W�����������A���@�U **[�U�@�B: ���� >]**�C

1. �Ъ`�N�A�w�g�]�w�w�]���|�C

    * �W�١G**route**
    * �ȡG`FROM /messages/* INTO $upstream`

    �����ѷ|�N IoT Edge �˸m�W�Ҧ��Ҳժ������T�����ǰe�� IoT ����

1. ���@�U **[�˾\ + �إ�]**�C

1. �b **[���p]** �U�A�Ъ�@�������ɶ��A�˾\��ܪ����p��T�M��C 

    �p�z�Ҩ��AIoT Edge �˸m�����p��T�M��|�榡�Ƭ� JSON�A�]���۷�e���\Ū�C

    `properties.desired` �Ϭq���U��O `modules` �Ϭq�A���Ϭq�ŧi�n���p�� IoT Edge �˸m�� IoT Edge �ҲաC�o�]�A�Ҧ��Ҳժ��M�� URI�A�]�A����e���n���{�Ҧb���C

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

    JSON ���U�b���O **$edgeHub** �Ϭq�A�䤤�]�t Edge ���ϩһݪ��ݩʡC���Ϭq�]�]�t�i�Ѧb�Ҳդ������Ѩƥ�A�H�θ��Ѧ� IoT ���Ϫ����ѲպA�C

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

    JSON ���U�b���٦� **turbinesensor** �Ҳժ��Ϭq�A�䤤�� `properties.desired` �Ϭq�]�t Edge �Ҳժ��պA�һݪ��ݩʡC

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

1. �Y�n�����]�w�˸m���ҲաA�Цb�M�W���������A���@�U **[�إ�]**�C

1. �Ъ`�N�A�b **turbine-06** �M�W������ **[�Ҳ�]** �U�A�{�b�w�C�X **turbinesensor**�C

    > **�`�N**�G�z�i�ॲ�����@�U **[���s��z]**�A�~��ݨ�Ĥ@���C�X���ҲաC

    �z�i��|�`�N��å��^�� **turbinesensor** �� [���涥�q���A]�C

1. �b�M�W�������ݡA���@�U **[���s��z]**�C

1. �Ъ`�N�A**turbinesensor** �Ҳժ� **[���涥�q���A]** �{�b�]�w�� **[���椤]**�C

    �p�G�����^���ȡA�еy�Ԥ���A�M��A�����s��z�M�W�����C
 
1. �}�� Cloud Shell �u�@���q (�p�G�|���}�Ҫ���)�C

    �p�G�z���A�s�u�� `vm-iot-edge-{deployment-id}` ���������A�Цp���`�@�˨ϥ� **SSH** �ӳs�u�C

1. �Y�n�C�X�ثe�b IoT Edge �˸m�W���檺�ҲաA�Цb Cloud Shell �R�O���ܦr����J�U�C�R�O�G

    ```cmd/sh
    iotedge list
    ```

1. �R�O����X�ݰ_�өM�U�������C 

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME             STATUS           DESCRIPTION      CONFIG
    edgeHub          running          Up a minute      mcr.microsoft.com/azureiotedge-hub:1.0
    edgeAgent        running          Up 26 minutes    mcr.microsoft.com/azureiotedge-agent:1.0
    turbinesensor    running          Up 34 seconds    asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ```

    �Ъ`�N�A`turbinesensor` �w�C���䤤�@�Ӱ��椤���ҲաC

1. �Y�n�˵��ҲհO���A�п�J�U�C�R�O�G

    ```cmd/sh
    iotedge logs turbinesensor
    ```

    �R�O����X�ݰ_�өM�U�������G

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge logs turbinesensor
    11/14/2019 18:05:02 - Send Json Event : {"machine":{"temperature":41.199999999999925,"pressure":1.0182182583425192},"ambient":{"temperature":21.460937846433808,"humidity":25},"timeCreated":"2019-11-14T18:05:02.8765526Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.599999999999923,"pressure":1.0185790159334602},"ambient":{"temperature":20.51992724976499,"humidity":26},"timeCreated":"2019-11-14T18:05:03.3789786Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.999999999999922,"pressure":1.0189397735244012},"ambient":{"temperature":20.715225311096397,"humidity":26},"timeCreated":"2019-11-14T18:05:03.8811372Z"}
    ```

    `iotedge logs` �R�O�i�H�Ψ��˵����� Edge �Ҳժ��ҲհO���C

1. �����ū׷P�����Ҳշ|�b�ǰe 500 �h�T�����ᰱ��C����U�C�R�O�Y�i���s�ҰʡG

    ```cmd/sh
    iotedge restart turbinesensor
    ```

    �z���ݭn�ߧY���s�ҰʼҲաA���p�G�z�y�Եo�{�Ҳհ���ǰe�����A�Ъ�^ Cloud Shell�ASSH �� Edge VM�A�M����榹�R�O���H���]�C���]����A�Ҳշ|�A���}�l�ǰe�����C

### �u�@ 3�G���p Azure ��y���R�@�� IoT Edge �Ҳ�

�{�b�A**turbinesensor** �Ҳդw���p�b IoT Edge �˸m�W�óB�󥿦b����A�ڭ̥i�H�s�W��y���R�ҲաA�p���i���b IoT Edge �˸m�����B�z�T���A�M��A�ǰe�� IoT ���ϡC

#### �˾\��y���R�@�~

1. �b�귽�s�տj�W�A���@�U **iot-{deployment-id}**�A�M������y���R�@�~�W�� **iotedge-streamjob-{deployment-id}**�C

1. �M��b�M�W���������� **[�@�~�ݼ�]** �U�A��� **[��J]**�A�M��T�{�w�w�q�@��**�ū�**��J�u�@�C

1. ���ۡA��� **[�@�~�ݼ�]** �U�� **[��X]**�A�M��T�{�w�g�w�q**ĵ��**��X�u�@�C

1. �b���������\��� **[�]�w]** �U�A���@�U [�x�s��b��]�w]�C�M��A�T�w�w�g�s�W iotstorage{deployment-id} �x�s��b��C

#### ���p��y���R�@�~

1. �b Azure �J�f�������A�s���� **iothub-{deployment-id}** IoT ���ϸ귽�C

1. �b���������\��� **[�۰ʸ˸m�޲z]** �U�A���@�U **[IoT Edge]**�C

1. �b **[�˸m�ѧO�X]** �U�A���@�U **turbine-06**�C

1. �b **turbine-06** ���泻�ݡA���@�U **[�]�w�Ҳ�]**�C

1. �b **[�b�˸m�W�]�w�Ҳ�: turbine-06]** ����W�A��� **[IoT Edge �Ҳ�]** �Ϭq�C

1. �b **[IoT Edge �Ҳ�]** �U�A���@�U **[�s�W]**�A�M����@�U **[Azure ��y���R�Ҳ�]**�C

1. �b **[Edge ���p]** ���檺 **[�q�αb��]** �U�A�T�w�w����󦹽ҵ{�ϥΪ��q�αb��C

1. �b **[Edge �u�@]** �U�Ԧ��M�椤�A�T�w�w��� **iotedge-streamjob-{deployment-id}** ��y���R�@�~�C

    > **�`�N**�G�z�i��w�g����Ӥu�@�A�� **[�x�s]** ���s�B�󰱥Ϊ��A - �u�n�A���}�� **[Edge �u�@]** �U�Ԧ��M��A�æA����� **iotedge-streamjob-{deployment-id}** �u�@�C���|�ҥ� **[�x�s]** ���s�C

1. �b���橳���A���@�U **[�x�s]**�C

    ���p�i��ݭn��O�@�Ǯɶ��C

1. �b **[�b�˸m�W�]�w�Ҳ�: turbine-06]** �M�W���������A���@�U **[�˾\ + �إ�]**�C

1. �b **[�˾\ + �إ�]** ���޼��ҤW�A�Ъ`�N**���p��T�M��** JSON �{�b�|��s����y���R�ҲթM��]�w�����ѩw�q�C

1. �b�M�W���������A���@�U **[�إ�]**�C

1. ���\�o�G Edge �M�󤧫�A�Ъ`�N�s�� ASA �ҲմN�|�C�b **[IoT Edge �Ҳ�]** �Ϭq�U��C

1. �b **[IoT Edge �Ҳ�]** �U�A���@�U **iotedge-streamjob-{deployment-id}**�C

    �o�O��~�s�W�� Edge �˸m����y���R�ҲաC

1. �Ъ`�N�A**[��s IoT Edge �Ҳ�]** ����W�� **[�M�� URI]** ���V�зǪ� Azure ��y���R�M���C

    ```text
    mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.7
    ```

    �o�O���p�� IoT Edge �˸m���C�� ASA �@�~�A���|�ϥΪ��ۦP�M���C

    > **�`�N**�G  ��z�إߦ�y���R�ҲծɡA**[�M�� URI]** �������������X�]�w���ϬM�ثe���̷s�����C

1. ���Ҧ��ȳ�������w�]�ȡA�M������ **[IoT Edge �ۭq�Ҳ�]** ����C

1. �b **[�b�˸m�W�]�w�Ҳ�: turbine-06]** �M�W�����W�A���@�U **[�U�@�B: ���� >]**�C

    �Ъ`�N�A�{�������H�Y��ܡC

1. �ХH�U�C�T�Ӹ��Ѩ��N�w�]�ҩw�q�����ѡG
   
   > **�`�N**�G�аȥ��H Azure ��y���R�@�~�Ҳժ��W�٨Ө��N `iotstreamjob-edge-{deployment-id}` �w�d��m�C
   
    * ���� 1
        * �W�١G**telemetryToCloud**
        * �ȡG`FROM /messages/modules/turbinesensor/* INTO $upstream`
    * ���� 2
        * �W�١G**alertsToReset**
        * �ȡG`FROM /messages/modules/iotedge-streamjob-{deployment-id}/* INTO BrokeredEndpoint("/modules/turbinesensor/inputs/control")`
    * ���� 3
        * �W�١G**telemetryToAsa**
        * �ȡG`FROM /messages/modules/turbinesensor/* INTO BrokeredEndpoint("/modules/iotedge-streamjob-{deployment-id}/inputs/temperature")`

    > **�`�N**�G�z�i�H���@�U **[�W�@�B]**�A�˵��ҲղM��P�U�Ҳժ��W�١A�M��� **[�U�@�B]**�A��^���B�J�C

    �ҩw�q�����Ѧp�U�ҥܡG

    * **telemetryToCloud** ���ѷ|�N�Ӧ� `turbinesensor` �Ҳտ�X���Ҧ��T���ǰe�� Azure IoT ���ϡC
    * **alertsToReset** ���ѷ|�N�Ӧۦ�y���R�Ҳտ�X���Ҧ�ĵ�ܰT���ǰe�� **turbinesensor** �Ҳժ���J�C
    * **telemetryToAsa** ���ѷ|�N�Ӧ� `turbinesensor` �Ҳտ�X���Ҧ��T���ǰe�ܦ�y���R�Ҳտ�J�C

1. �b **[�b�˸m�W�]�w�Ҳ�: turbine-06]** �M�W���������A���@�U **[�˾\ + �إ�]**�C

1. �b **[�˾\ + �إ�]** ���޼��ҤW�A�Ъ`�N**���p��T�M��** JSON �{�b�|��s����y���R�ҲթM��]�w�����ѩw�q�C

1. �Ъ`�N�A`turbinesensor` �����ū׷P�����Ҳժ� JSON �պA�G

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

1. �Ъ`�N�A���e�ҳ]�w���Ѫ� JSON �պA�A�H�Φp��H JSON ���p�w�q�ӳ]�w�G

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

1. �b�M�W���������A���@�U **[�إ�]**�C

#### �˵����

1. ��^ **Cloud Shell** �u�@���q�A�z�b���B�z�L **SSH** �s�u�� **IoT Edge �˸m**�C  

    > **�`�N**�G�p�G�s�u�w�����ιO�ɡA�Э��s�s�u�C���� `SSH` �R�O�A�éM���`�@�˦a�n�J�C

1. �Y�n�˵����p�ܸ˸m���ҲղM��A�Цb�R�O���ܦr����J�U�C�R�O�G

    ```cmd/sh
    iotedge list
    ```

    �ݭn��@�I�ɶ������N�s����y���R�Ҳճ��p�� IoT Edge �˸m�C��������A�Y�i�z�L���R�O�b�M���X���ݨ�ӼҲաC

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME                       STATUS           DESCRIPTION      CONFIG
    iotedge-streamjob-232539   running          Up a minute      mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5
    edgeAgent                  running          Up 6 hours       mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                    running          Up 4 hours       mcr.microsoft.com/azureiotedge-hub:1.0
    turbinesensor              running          Up 4 hours       asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ``` 

    > **�`�N**�G  �p�G�M�椤����ܦ�y���R�ҲաA���R�Ԥ@�ܤG�����A�M��A�դ@���C�ݭn��@�I�ɶ������b IoT Edge �˸m�W��s�Ҳճ��p�C

1. �Y�n�[�ݥ� `turbinesensor` �Ҳձq Edge �˸m�ǰe�������A�Цb�R�O���ܦr����J�U�C�R�O�G

    ```cmd/sh
    iotedge logs turbinesensor
    ```

1. �Ъ�@�������ɶ��A�[��@�U��X�C
 
    ���ƥ󪺿�X�ݰ_�өM�U�������G

    ```cmd/sh
    11/14/2019 22:26:44 - Send Json Event : {"machine":{"temperature":231.599999999999959,"pressure":1.0095600761599359},"ambient":{"temperature":21.430643635304012,"humidity":24},"timeCreated":"2019-11-14T22:26:44.7904425Z"}
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":531.999999999999957,"pressure":1.0099208337508767},"ambient":{"temperature":20.569532965342297,"humidity":25},"timeCreated":"2019-11-14T22:26:45.2901801Z"}
    Received message
    Received message Body: [{"command":"reset"}]
    Received message MetaData: {"MessageId":null,"To":null,"ExpiryTimeUtc":"0001-01-01T00:00:00","CorrelationId":null,"SequenceNumber":0,"LockToken":"e0e778b5-60ff-4e5d-93a4-ba5295b995941","EnqueuedTimeUtc":"0001-01-01T00:00:00","DeliveryCount":0,"UserId":null,"MessageSchema":null,"CreationTimeUtc":"0001-01-01T00:00:00","ContentType":"application/json","InputName":"control","ConnectionDeviceId":"turbine-06","ConnectionModuleId":"vm-iot-edge-CP1119","ContentEncoding":"utf-8","Properties":{},"BodyStream":{"CanRead":true,"CanSeek":false,"CanWrite":false,"CanTimeout":false}}
    Resetting temperature sensor..
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":320.4,"pressure":0.99945886361358849},"ambient":{"temperature":20.940019742324957,"humidity":26},"timeCreated":"2019-11-14T22:26:45.7931201Z"}
    ```
 
1. �Ъ`�N�A�b�[�ݥ� **turbinesensor** �ǰe���ū׻����ɡA�� `machine.temperature` �������W�L `72` �ɡA��y���R�@�~�N�|�ǰe **reset** �R�O�C�o�N�O�b��y���R�@�~�d�ߤ��]�w���ʧ@�C

�z�w�b���m�ߤ��Q�� Azure IoT Edge �A�ȡA�b Edge �˸m�W�B�z�T���C
