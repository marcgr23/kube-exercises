EXERCISE 1
==========

ReplicaSet:
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-exercise
  labels:
    app: nginx-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-server
  template:
    metadata:
      labels:
        app: nginx-server
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.19.4
        resources:
          limits:
            memory: "128Mi"
            cpu: 20m
          requests:
            memory: "128Mi"
            cpu: 20m
```

Service:
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx-deployment
  ports:
    - port: 80
      targetPort: 80
```

Ingress:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-server-ingress
spec:
  tls:
  - hosts:
      - marc.student.lasalle.com
    secretName: nginx-secret
  rules:
  - host: marc.student.lasalle.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
```

Secret con las credenciales de nuestro certificado SSL:
```
apiVersion: v1
kind: Secret
metadata:
  name: nginx-secret
type: kubernetes.io/tls
data:
  tls.crt: 
    MIIEwjCCAqoCCQD7zIZrwBwbQTANBgkqhkiG9w0BAQsFADAjMSEwHwYDVQQDDBhtYXJjLnN0dWRlbnQubGFzYWxsZS5jb20wHhcNMjAxMTE4MTMxMDI5WhcNMjExMTE4MTMxMDI5WjAjMSEwHwYDVQQDDBhtYXJjLnN0dWRlbnQubGFzYWxsZS5jb20wggIiMA0GCSqGSIb3DQEBAQUAA4ICDwAwggIKAoICAQCwXNLE411+SH/l8fQORZMLLyF9sw+U3sFsmSKjEZhcEEVvXCPzncg1hUaaKpJMlj7Uha+SG1Ny5EVBqeVRYzLB9i+kYjjcdS/7iBG8AoQg9QGxtulKQg0DPxPX3hw7PgOx1ewnGOGURIG5LpuekcmHMypPcqy469bFEBpHsBzSNybf0BN5Ev2wST4/QV2JA5Dd4m0luWuStg+0fuhfV7f1oWjHOq8uPrp0zLyLaiLnBVzNjkJRE3akajJoUMuaX1UzVsGXDys+gAE9VcCatqQl480W+5NXLDSCEczdxx3uDN9q5auP9RS+ky63S6/ds3PmE/aPb3sUT9eJ1bSuctQYEf/DcBXlg9NOzJP1sV856Qhz7pBzM2jPrjTvZun1fJLfMOmzKTW01krCR2/h7u23XGi2BZuf74xu0RrvPC74CnKJYaA7wNJn8ZASjzZFaMF2W7o0R6/aYPifjzSQ0ew3ArqkBH7AlOKxv71Qu3uklQFHFmTvhWfmi70g2NC7VF22jED7GNl60MsG0O3UFqDNUTGWuy1tG+manT31PVZ2sHWAEgliGePMalyHTQ0aWrDeSZn0KcwIfe9LzWsd4pY4zJnNY7fBeFGaJ0hTb0UFhly9f7e8lLLoi4grK5QmZRT1H8l+PsqsRFEILYEomdDDNdMSQw8HzdikiZ/4R94fIwIDAQABMA0GCSqGSIb3DQEBCwUAA4ICAQBC/o628zzqE7PAhC8FLEGA/HZ0q0C+1x6Dt3seMVY0cC5O0WTPBAEhc2sbmyb/x8otGzk1n7/bQs7wPI032F0WpM/cxys6X+lGhWrQNRweR2IlLxn5+eKx/V4dEk26KeyvjO+z7vIlajZwP3l7PN6qu0Y2r2+h+yY+TFylA+yjPux9nIqlBU+mT2rXNZGAn6JhUM/qWY1QoX4qeggPYbcl4cPDrR+Hf9GglMoXIPur/MhJfpJ30W4qHvJh9oDtoy9FlhHlTT9wL9Wv7gEHw0DcnGOFrMXrc/ahCkksskPqVYvzRExl6+fBoPNxfA90PHo+6yUVdbRNMTp3qkP/WIZvvRN7zMRSbnFKyJLQfItUBg9AnlKM8UbItkbxUyMHqunay4Yl3JqRTAPQeoJNK7j08qe4yNZY3qnEwwcQKVZEq4Lw4K5kcJt9uopNEQtan3VoSiA4H/DcFSc4AoQL4h2MyHBCvSIH5uyHHHAZCavgMAJSvJemm6c4yBVLOqfvwEP3vsdep7sNp4OE0T7ptV6psMEHOWHXk9YRyCQaDk79IB7daRjNhecfInkzkDLpTp96UQnpUqfHi0dfxa8P2QFXAXVtPSX5thLTuEnXc2yO6E93mCpATPrG7zdlae11glD49j05apGIaQsJSY6yM1HhmkadZxTUEdU1d/6wfJ2CcA==
  tls.key: 
    MIIJnzBJBgkqhkiG9w0BBQ0wPDAbBgkqhkiG9w0BBQwwDgQIum1gilmVud0CAggAMB0GCWCGSAFlAwQBKgQQd1+PVirQ+3PyXjWd9ZJErASCCVBqHhciCQRO8UpfQc91b0xLMAlcDReks1FsJXq232m6V7NphtNqp8/NAJDHFbX8g2e/3Q20cq2GZ4uK1DEWk/jD7AwoDVQHouk+7I45RKHheNPn+ISDE3y41BUA2uGNmondFWWdXpssCBdlYp8cR6QPayG/pOdR1RNJloQR/NOiOX9JXuvtFi3HRtklKpzJcyRbehbIWopvAjCaxiJfYdb3d7j3EaJrkYIuuagbLY+2OzSucMwR5KnbbQX6cAxGMdIhrigeM9SNnfEcSNMz9j5Bmm30ENUpePYOF+UMlcIFbYoiC5aBG1jNqv9XPMJoAQiM6Ep5VET0lybDnmfteczky+FhnCftWRUg4Erw9HGMyDKHjYenTAJbR/SKYJvEHXbtW6SHWwFjtfaZpvQVQ9V+AfZ8GzmWSUAlixnVujtA7uHFdctKmRSJAQuh19X+n7t6wnavtj88k4N0Tk9xKAWdcWqmMjg2+HryXwHMjlCu0dJPpDEtfQZPr/B6RbtR0fAJXYfN40RbVsILaTQSTeZxA2lwHzLy4S8F9wSfyWF8H9ywM17sxZooldUZBP3awZ9xUaLP+jXxYSByS8vj6S1zndxGN68dw7YozYtg56S97UW+voWNpsHQatdZMvgG3yC/pjjGl/2Yp3PeJAO5SQlWmvundrP0Gyk2UQaR1xXpiagONgdw9fca9POxKAXEWIzGE8P4IAFbGJk4uPMVejGnLEytXN4I1/hL2hPIPFX9/SrcAuFBP0Lk6uPApFihP3sV9KLg/sgAFXiBhu7J/zccHGLyr4nWvqOO9XRLIKCRhLBfcAE4jzcfPP51WdHPgpLX9VPUGfi0darqDLdMlZ52E02TLyUA6t7tPoLoS+uWAZJ6T8UzxHFebnWS6agrVc2pNLF9qicRAetvDcEX+zD7NwxmhvfT92ysK1dJ48Ztz3F17bLxznQ3VkP7A0ax70f/XYlSnw3XjwFL/+Vy7EvDBBZ7O9TMWQuPRuJTmTld4ThWjqwtBQ3xiyvwQRpP6KfLhmP4KiuA5wvwzmT5rx0OPgJVVKP9b4O0MrB3HFgmzQv0O80j48s20+pJPx1v2BCQGpNf8G1ly2gGd1zIXcxJlUQYxn3ALCydG7ROzLZenp9FiZTFhoAAMC+Ps7jnrJ+8eW2v64VyLILFhxeRFRxmgWJrQ19PPA4Z/vhRhqf0YgwwEDSHQHZbKe1HQjFo00MbWdP2T4c6EY1IjKrJK2qRyerYTLZQNjuCf5M/6k2+JbnmTTOUcbFDLlNjBiWAjwDwRV6sU4w1dom8UrxiVh+PBRZuNXuedr2h3x705OIt4teKDX5ugXrLF/8jSFALgfhx/dNeoJEptx2k4026A83ar44bN/HFw85JlmgzjnTfc969MLTIza7oU0M9SeZ/946SdqyIqcPdLSb5lSLm/MaBVRAtcouyzMwziKmJ6HyvPOcby3LJ/qrOsavHg824W0Z9MpBSTSLz4emK6ZnBEucH780LxS7A/BqyE4q9tXUrfEXQWUBeKt4BqeuBTWaNIv0uEBJyRcJvB9HLWVVk5SRaQBSSoQEpAkJpBGeb2RvuHPjTQui2+J2rXHYk+c2ZDDmPUMOJFSI3QbMki2eI4L/nlXrlpL1giIRPJi29A1cd19IqITVbPICHIYu1VHEyRfyEsLGLIGXS4xr6rk2Aa9hzCXadM24t07hN1s8Kbg6UgUtWP5IwnwH/5hiAPTYeqKh0tizKvC2EuBEhl8LcuoFraaNB4mJChOTBFGcgAgtRsBKN8ikoIs+9NqKIuKDSlOSWnwmxbGcmHJ7S3epQjVlcr382YBystRYVO/jyAOuJXEyUz5+hobJ5xEhFBphdsepzVqx5kFP+Euwv+wwBPfOeDT3/XJb6InCgOoBcDOrBEd8pGTwLVILWlksfzJshbBQqOw+EbZoxPw+oAFhWfYpNhwXtIKYuZcrn1IwFI18RWW62ZLnURzVuo2TRxdebYJvpFp/5639LVxfOfpoc6qtv1Z6ODLAGe2rtgkSMNYvO/GN2LIRC82SAS5dd3UhMPvwt1cWnND/DvyqR2HqzA3lYEJ+/1f+vr79eQ2CY0a4cnbxOTd0uqQxP+Jnx1AmAYN75wFx545t67TQ1HrqSCGmZz59oqAhEaowr0x+ndXCNWcFZSga+UhMjL9XPzNmvzIrcnpY6x0Gby2xbGXJCgIzg87j3Ff96YVBV9IQXZ6Y6E6fZUN43r/J+rEtMfrI/K6wUJhoTBGCwW6KP/CfGq25XFlspv0yoJsHyZ03zlsGaqVL+wMsTA7OXMs9/q5OfK5QRE0i8bmg93zJZG+2VyH0CZF35848j+ZU+rpcKUH6AyKmrfq8Y+59GpphEdr1aqHkIMmbB9mcDjgfEJdA2YE85whgrty+0IGYdaI81UJSht/oZA2mvXFNV/ETJiJUmupBM9UtNESK0x16zLi7XagfuFxfw3uc9vlivG4iQ83zydRwR8hSiT6grfTG915U0OAsGWgfdJelHIBExvojwatqfEizcuX3+8C3QXnSHZeHvl9UX4uNShYMSZ2b/+kZ+r+EJE7n/rIFEdO/5d0+MmS1g1TNXUznDjej31pIYq55TnADij6AXLsrfmd+12+VpK3gy34o/GKwWjxXEszgYkbPASGPhRcDIt6LYNfsAGJhpsn6w6nnqA/t65KNFaSBFVBp/FxwmPhlK+Y80veUjLvEOsIJGsHdTtSY0A3jbwLtt1q/XEODEfiNjfQzHtXQb/ap53qYAhg3NgWscVV6WvwUNQ5ElyUxidvoiNA9cN+LcMz3dFXTA6zPjrx1Gpgcx34rpw+07Enx52eFLUH4Dz5JT0CbtqJ8/R8itsIPUIETJU1h/3FqjGCZRB2+uHmJPtxSxWQnd/s+u1j3X+WPRoQtwIdB4x+4p0OnVMQB3+YFxNUbOdC+KCizdeU48LFa1uDmCdhEhlBIawOtElMT2Bgib58mbDuqSu2DgmufFtrxaYypCafWWcXWsoe4j60+LNXbORNnu8sEH4EKQQ4uexTBIyK50Ia1Q8gdvaXWhZXGDYdimI+S4liX/4vY0Lq16DYD04B+6cUsim0hwJ85K/5irFHEU1I5hUeQsuFN6nScid0LLhXGiJf8VJ2hmqIcTrDh4oojjPAmjFrGBaLCYuwgAL8BttfH3GrFInSHrFbjLTA==
```

Se necesita habilitar ingress en minikube para el uso de Ingress:
```
minikube addons enable ingress
````

Añadir ip en archivo hosts
```
192.168.99.100 marc.student.lasalle.com
```

Crear certificado SSL:
```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -subj '/CN=marc.student.lasalle.com'
```

Certificado SSL instalado en nuestro Secret:
![Carga](images/credenciales.png)