# Instruction for Colony Chain Developers

이 문서는 콜로니 체인을 개발하기 위한 가이드를 제시합니다.

기본적인 내용은 일단 `pbc-colony-common` crate와 `pbc-contract-common`을 **필수적으로** 참고해주세요.

콜로니 체인이 정상적으로 PBC 프레임워크 아래에서 작동하기 위해서는 아래의 작업들이 필요합니다.
각각의 태스크에 대해 자세한 스펙은 crate에 들어있는 Doc comment를 참고해주세요.

1. Light Client 컨트랙트 개발 및 디플로이: 자세한 스펙은 crate를 참고해주세요.
2. Treasury 컨트랙트 개발 및 디플로이: 자세한 스펙은 crate를 참고해주세요.
3. `trait ColonyChain` 구현

## Light Client 컨트랙트
자세한 스펙은 해당 crate를 참고해주세요.

### 인터랙션
Treasury 컨트랙트를 비롯한 다른 컨트랙트에서 Light Client를 참조할 수 있도록 인터페이스를 설계해놓는 것이 중요합니다.
컨트랙트간 인터랙션은 체인마다 상이할 것으로 예상되니 조사가 필요합니다.

### 검증로직과 내부상태
검증로직과 내부상태는 `Simperby`가 정의합니다. `pbc-contract-common`에 있는 `struct LightClient`를 사용하면 되고 그 이상으로 직접 알아야할 내용은 없습니다.

## Treasury 컨트랙트
자세한 스펙은 해당 crate를 참고해주세요.

### 인터랙션
Light Client 쪽으로 검증을 요청해야하는 경우에 적절히 인터랙션을 필요로 합니다. 마찬가지로 체인마다 상이하니 조사가 필요합니다.

### 토큰 모델
체인마다 토큰에 대한 모델과 스펙 (EVM의 ERC20 등)은 상이할 것입니다.
- Native, 혹은 first-class 토큰 (Etheruem의 Ether 등)이 특별취급 될 경우 이를 고려해야합니다.
- NFT의 경우 collection 내에서 토큰 index를 지정하는 방식을 조사해야 합니다.

### 보안
토큰이 오고가는 컨트랙트인 만큼 기본적인 보안사항을 지켜아합니다. (추후 엄격한 3자 검증을 할 예정이긴 합니다)
- Reentrancy attack와 같은 유명한 취약점들은 조사해보고 해당하는지 알아봅시다
- 각 체인의 VM에 specific한 내용들은 추가로 조사해봅시다. (세미나도 좋습니다)

## ColonyChain 구현
`trait ColonyChain`을 구현함으로써 디플로이된 상기 두 컨트랙트와 상호작용을 할 수 있습니다.
자세한 스펙은 crate를 참고해주세요.

- 구현체는 본 저장소가 아닌 각 체인의 저장소에 올라가야 합니다.
- 테스트를 위해 `Custom` 컨트랙트를 하나 개발 해보는 것도 권장합니다.

### 초기화 및 상태
`trait ColonyChain`에 명시되지 않은 내용, 예컨대 상태나 초기화 함수등은 자유롭게 정의하시면 되지만 기본적으로
1. 계정정보
2. 풀노드 URI
3. 디플로이된 컨트랙트 주소
정도의 정보로 충분할 것입니다.

