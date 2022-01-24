# SlimeVR-Tracker-STM32-nRF24
SlimeVR-Tracker-STM32-nRF24 프로젝트의 목적은 기존 ESP8266/ESP32로 구현된 [SlimeVR-Tracker-ESP](https://github.com/SlimeVR/SlimeVR-Tracker-ESP) 프로젝트를 STM32 마이크로컨트롤러와 nRF24 무선 통신 칩으로 포팅하는 것입니다.

## Why STM32 + nRF24?
ESP8266/ESP32는 저렴한 가격에 고성능 프로세서와 WiFi 통신기능을 갖고 있어 센서 신호를 처리하고 WiFi 네트워크로 전송할 수 있으며, Arduino IDE나 PlatformIO를 이용해 쉽게 프로그래밍 할 수 있어 널리 사용되고 있습니다.

하지만 다음과 같은 단점이 있습니다.
- 사용할 수 있는 GPIO 및 AFIO 수가 적어 구현할 수 있는 기능이 한정되어 있습니다.
- 전력 소모가 커 배터리 사용시간이 적습니다.
- 외부 플래시 메모리가 필요하여 많은 공간이 필요합니다.

이와 같은 단점을 해결해보고자 STM32 마이크로컨트롤러와 nRF24 무선통신 칩으로 같은 기능을 구현해보고자 합니다. 기대할 수 있는 점은 다음과 같습니다.
- STM32는 시리즈가 많아 커스터마이징 할 수 있는 폭이 넓어 보다 유연한 개발이 가능합니다.
- 저전력 프로세서이므로 ESP8266/ESP32보다 긴 배터리 구동 시간을 확보할 수 있습니다.
- USB 통신이 가능한 STM32의 경우, 시리얼 통신 및 펌웨어 업로드를 위해 별도 트랜시버가 필요하지 않아 공간 확보에 유리합니다.

## System Map Comparison
### 기존
[ESP8266/ESP32] - [WiFi Router] - [PC] - [SlimeVR Server]

### 이번 프로젝트
[STM32] - [nRF24] - [nRF24] - [STM32] - [USB] - [PC] - [node.js] - [SliveVR Server]

SlimeVR 서버가 소켓 통신을 사용하므로 nRF24를 통해 수신한 데이터를 소켓 통신으로 전환하는 소프트웨어를 node.js 기반으로 제작합니다.
nRF24는 SPI 통신을 사용하므로, PC에서 사용하기 위해 시리얼 통신으로 전환합니다.

## Development Process
1. \[Tracker\] Arduino Nano(328p)와 nRF24L01 모듈, MPU-6050 IMU를 이용하여 테스트 회로를 만듭니다.
2. \[Tracker\] WiFi와 관련된 함수를 nRF 통신으로 우회하는 라이브러리를 만듭니다.
3. \[PC\] node.js를 이용하여 UDP 통신을 시리얼 통신으로 전환하는 프로그램을 만듭니다.
4. \[Dongle\] nRF24를 PC에서 사용할 수 있게 시리얼 통신으로 제어가 가능하도록 동글을 만듭니다.
5. 위 과정 완료 후 1:1 통신을 테스트합니다. 대역폭 및 반응속도 등을 측정한 후, 네트워크 스택 사용 여부를 결정합니다.
6. \[PC\] 다중 장치를 연결할 수 있게 프로그램을 수정합니다.
7. \[Tracker\] Arduino Nano에서 구현한 프로그램을 STM32에 포팅합니다.
8. \[Tracker\] 총 다섯 개를 만들고 최종 테스트를 합니다.

## IMPORTANT
근데 이거 만들 시간이 없어요

## Misc.
