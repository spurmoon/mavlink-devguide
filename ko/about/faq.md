# 자주 묻는 질문

## 사용자

<dl>
  <dt>MAVLINK는 얼마나 효율적인가요?</dt>
  <dd>MAVLINK는 매우 효율적인 프로토콜입니다. MAVLINK 1은 시작 신호(start sign)와 패킷 손실 탐지(packet drop detection)를 포함해 패킷당 8 바이트의 오버헤드를 가집니다. Mavlink 2는 14 바이트(부호를 포함하는 경우, 27 바이트)의 오버헤드를 가지지만, 훨씬 더 확장성 있는 프로토콜입니다.</dd>

  <dt>MAVLINK가 동시에 얼마나 많은 기기를 지원하나요?</dt>
  <dd>1부터 255까지의 범위(0은 유효한 ID가 아닙니다)의 시스템 ID를 가지는 총 255개의 기기를 지원합니다.
    <br><b>참고:</b> 엄밀히 말하자면, MAVLINK는255개의 동시 시스템을 지원합니다. 이러한 시스템은 여러 기기의 혼합, 지상국(GCS, Ground Control Station), 안테나 트래커와 기타 하드웨어를 포함할 수 있습니다.</dd>

  <dt>MAVLINK는 어떤 장치에서 사용 가능한가요?</dt>
  <dd>MAVLINK는 ARM7, ATMega, dsPic, STM32등의 마이크로컨트롤러와 Windows, Linux, MacOS, Android와 iOS등의 운영체제에서 작동하는 것으로 보입니다.</dd>

  <dt>MAVLINK를 얼마나 신뢰할 수 있나요?</dt>
  <dd>상당히 신뢰할 수 있습니다. MAVLink는 다양하고 까다로운 통신 채널(높은 지연율/잡음) 환경에서 다양한 기기와 지상국(및 다른 노드) 간 통신을 위해 2009년부터 사용되었습니다. MAVLINK는 패킷 손실 탐지와 패킷 결성 체크를 위해 잘 알려진 ITU X.25 체크섬 메서드를 사용합니다.</dd>
  
  <dt>MAVLINK는 얼마나 안전한가요?</dt>
  <dd>MAVLINK는 시스템이 신뢰할 수 있는 출처에서 오는 메시지임을 인증하도록 <a href="../guide/message_signing.md">message signing</a>을 사용합니다. MAVLINK는 메시지 암호화를 제공하지 않습니다.  
  </dd>
</dl>

## 개발자

<dl>
  <dt>MAVLINK를 소스 코드를 공개하지 않는 어플리케이션에서 저작권 문제 없이 사용할 수 있나요?</dt>
  <dd>어떠한 제한 없이 MAVLINK를 사용할 수 있습니다. 생성된 MAVLINK 라이브러리 헤더는 *MIT 라이센스*(자세한 정보는 <a href="../README.md#license">Introduction > License</a>를 참조하세요.)하에 사용할 수 있도록 만들어졌습니다.
  </dd>

  <dt>MAVLink는 어떻게 메시지를 감지하고 바이트 스트림으로 디코딩하나요?</dt>
  <dd>MAVLink는 패킷 시작 신호를 기다리다가 패킷의 길이를 읽습니다. 그리고 n 바이트 이후의 체크섬을 검사합니다. 만약 체크섬이 일치한다면, 디코딩된 패킷을 반환하고 다시 시작 신호를 기다립니다. 만약에 바이트가 변경되거나 손실되면, MAVLink는 현재 메시지 처리를 중단하고, 다음 메시지에서 디코딩을 계속합니다.</dd>

  <dt>MAVLink는 하나의 시작 신호만을 사용하는데, 두세개의 시작 신호를 사용하는 것보다 덜 안전하지 않나요?</dt>
  <dd>그렇지 않습니다. MAVLink는 CRC 체크를 통해 수신 메시지의 무결성을 판단합니다. 시작 신호를 추가로 사용하는 것은 시작 신호의 탐지율을 올릴 수 있지만, 메시지 유효성 관점에서는 단일 시작 신호에 비해 불확실성을 크게 줄이지 못합니다. 추가 시작 신호가 커뮤니케이션 링크의 바이트 수를 증가시키기 때문에, MAVLink는 단일 시작 신호를 사용하기로 결정했습니다.</dd>

  <dt>시스템 ID와 컴포넌트 ID는 어떤 용도인가요?</dt>
  <dd>시스템 ID는 특정 <em>MAVLink system</em>(vehicle, GCS, etc.)의 신원을 나타냅니다. MAVLink는 동시에 최대 255개의 시스템에서 함께 사용될 수 있습니다. 컴포넌트 ID는 더 큰 시스템의 요소를 나타냅니다. 예를 들면, 시스템은 autopilot, companion computer, 카메라 등 개별적으로 식별할 수 있는 장치를 포함할 수 있습니다. 따라서 컴포넌트 ID는 MAVLink가 온보드와 오프보드 통신 모두에서 사용되게 합니다.</dd>

  <dt>MAVLink 헤더의 시퀀스 넘버(Sequence number)는 왜 필요한가요?</dt>
  <dd>MAVLink는 무인 항공 시스템의 안전에 중요한 구성 요소의 일부입니다. 많은 패킷을 버리는 나쁜 통신 링크는 비행체의 안전을 위협할 수 있고, 모니터링이 필요합니다. 헤더에 시퀀스를 가지는 것은 MAVLink가 패킷 손실률에 관한 피드백을 지속적으로 제공할 수 있도록 하여 비행체나 관제국이 조치를 취할수 있도록 합니다.</dd>
  
  <dt>왜 CRC_EXTRA가 패킷 무결성 체크섬에 필요한가요?</dt>
  <dd>The CRC_EXTRA CRC is used to verify that the sender and receiver have a shared understanding of the over-the-wire format of a particular message 
  (required because as a lightweight protocol, the message structure isn't included in the payload).
  MAVLink 0.9에서는 CRC가 사용되지 않았습니다.(다만 길이 검사는 있었습니다.) 
  메시지를 설명하는 XML이 메시지 길이 변화 없이 변경되는 소수의 케이스가 있었는데, 이것은 메시지를 읽을 때 심각하게 손상된 필드로 이어졌습니다.</dd>

  <dt>디코딩/인코딩 루틴이나 다른 요소들을 개선하는데 도움을 주고 싶습니다. MAVLink가 바뀔 수도 있나요?</dt>
  <dd>매우 엄밀한 안전 테스트를 거치는 경우에 그럴 수 있습니다. 
  MAVLink는 많은 autopilot 시스템에서 중요한 안전 요소로 사용되기 때문에 다년간의 시험을 거칩니다. MAVLink <a href="../README.md#support">support channels</a>에 새로운 요소를 제안해주세요.</dd>
</dl>
