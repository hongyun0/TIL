## Timeout 설정하는 방법
Dockerfile에서 label 값을 설정해주면 서버와 클라이언트 타임아웃 시간을 조정할 수 있다. 

"labels": {   
      "HAPROXY_0_BACKEND_HEAD": "timeout server 30m\n",   
      "HAPROXY_0_FRONTEND_HEAD": "timeout client 30m\n"   
      }   
    
-> Timeout 30분으로 설정
