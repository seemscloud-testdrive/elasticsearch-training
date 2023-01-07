```bash
cat > values.yaml << "EndOfMessage"
esConfig:
  elasticsearch.yml: |
    cluster.name: "elastic"
    network.host: 0.0.0.0
    
    xpack.security.enabled: true
    
    xpack.security.http.ssl.enabled: true
    xpack.security.http.ssl.key: certs/tls.key
    xpack.security.http.ssl.certificate: certs/tls.crt
    xpack.security.http.ssl.certificate_authorities: certs/ca.crt
    xpack.security.http.ssl.verification_mode: certificate
    
    xpack.security.transport.ssl.enabled: true
    xpack.security.transport.ssl.key: certs/tls.key
    xpack.security.transport.ssl.certificate: certs/tls.crt
    xpack.security.transport.ssl.certificate_authorities: certs/ca.crt
    xpack.security.transport.ssl.verification_mode: certificate
EndOfMessage
```
