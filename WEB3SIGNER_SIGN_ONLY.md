# Web3Signer Sign-Only Mode Support / Web3Signer Sign-Only 모드 지원 안내

---

## [English] Web3Signer Sign-Only Mode Support

This version of `firefly-evmconnect` has been enhanced to support a native **Sign-only mode** when working with external signers like Web3Signer.

### Overview
Previously, connecting Web3Signer often required a "Proxy Mode" where the connector treated the signer as a node. This update allows `firefly-evmconnect` to explicitly delegate only the **signing** task to Web3Signer while keeping the **nonce management** inside FireFly (FFTM) and the **transaction submission** directly to the blockchain node.

### Key Changes
1. **`signer.url` Configuration**: A new configuration key to specify the external signer's endpoint.
2. **Automatic Signing Flow**: When a transaction is received from FFTM with `preSigned: false`, the connector calls `eth_signTransaction` on the external signer and then submits the signed raw data via `eth_sendRawTransaction`.
3. **Improved Multi-Chain Support**: Reliable signing across different networks by ensuring proper Chain ID injection during the signing process.

### How to Use
Update your `evmconnect.yaml` as follows:
```yaml
connector:
  url: http://besu-node:8545  # The actual blockchain node
  signer:
    url: http://web3signer:9000 # The external signer
```

---

## [한국어] Web3Signer Sign-Only 모드 지원 안내

본 버전의 `firefly-evmconnect`는 Web3Signer와 같은 외부 서명기를 사용할 때, 프록시 방식이 아닌 **서명 전용(Sign-only) 모드**를 공식적으로 지원하도록 개선되었습니다.

### 개요 및 배경
기존의 "프록시 모드"는 논스 동기화 문제와 성능 저하의 원인이 되기도 했습니다. 이번 업데이트를 통해 `firefly-evmconnect`가 **논스 관리 및 전송 주도권**을 직접 가지되, **서명 작업만** 외부 Web3Signer에 위임하는 구조를 실현했습니다.

### 주요 변경 사항
1. **`signer.url` 설정 추가**: 외부 서명기의 엔드포인트를 지정할 수 있는 새로운 설정 키를 도입했습니다.
2. **자동 서명 워크플로우**: FFTM으로부터 서명되지 않은 트랜잭션(`preSigned: false`)을 받으면, 외부 서명기에 `eth_signTransaction`을 요청하여 서명을 받아온 뒤 노드에 직접 전송합니다.
3. **EIP-155/1559 완벽 지원**: 서명 요청 시 Chain ID를 자동으로 파악하여 포함함으로써 최신 네트워크에서도 서명이 올바르게 수행되도록 보장합니다.

### 사용 방법
`evmconnect.yaml` 설정 파일에서 아래와 같이 노드 주소와 서명기 주소를 분리하여 설정합니다.
```yaml
connector:
  url: http://besu-node:8545   # 실제 블록체인 노드 주소
  signer:
    url: http://web3signer:9000 # 외부 서명기 주소
```

---
*Created on 2026-04-16 by Gemini CLI Agent for Keith-Digital.*
