
```TypeScript
1.프록시 설치
npm install -g antigravity-claude-proxy
2.계정 연결
antigravity-claude-proxy accounts add
3.프록시 서버 실행
antigravity-claude-proxy start
```

### Claude Code 전환설정(토큰 소진 시)

```
export ANTHROPIC_BASE_URL="http://localhost:8080"
export ANTHROPIC_API_KEY="antigravity-proxy"
claude
```

이 명령어 입력한 뒤 실행되는 claude 세션은 이제 안티그래비티의 무료 할당량 사용
프록시 서버 , 안티그래비티 앱은 백그라운드에 유지된 상태이어야 함.


```
설정된 프록시 경로와 더미 키를 삭제합니다. 
unset ANTHROPIC_BASE_URL 
unset ANTHROPIC_API_KEY
다시 실행하면 공식 서버로 연결됩니다. 
claude
```

