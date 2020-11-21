# Aggregate report
Article
- http://www.testingjournals.com/understand-aggregate-report-jmeter/

# Access REST API with client certificate
Articles
- https://www.blazemeter.com/blog/how-set-your-jmeter-load-test-use-client-side-certificates

#### Convert certificate to PKCS12 to JKS certificate

Use keytool of JDK to do the convertion
```
keytool -importkeystore -srckeystore certificate.p12 -srcstoretype PKCS12 -srcstorepass <certificate_password> -keystore <keystore_filename> -storepass <stored_password>
```
#### Load the certificate to JMeter
For UI mode, edit the `system.properties` in JMeter directory
```
javax.net.ssl.keyStore=<your_JKS_filename.jks>
javax.net.ssl.keyStorePassword=yourJKSpassword
```
For command line mode, add -D options direct to the command line
```
-Djavax.net.ssl.keyStore=your_JKS_filename.jks -Djavax.net.ssl.keyStorePassword=yourJKSpassword
```

#### TODO
- [ ] Need to check the SSL Manager in JMeter
> Using JMeter to Run Load Tests that Require Client Side Certificates
>
> JMeter supports client-side JKS and PKCS12 certificates ‘out-of-the-box’. It does this by using the SSL Manager to select the certificate when running in GUI mode. To use the PKCS12 certificate, make sure that the extension of the file is .p12 (e.g : mykeystore.p12). The other extension will be treated as a JKS (Java KeyStore) certificate.

# Extract the entire HTTP response string to variable
Articles
- https://stackoverflow.com/questions/34463412/how-to-save-response-in-a-variable-in-jmeter
- https://www.blazemeter.com/blog/how-use-beanshell-jmeters-favorite-built-component
- https://www.redline13.com/blog/2018/09/jmeter-extract-and-re-use-as-variable/

Under `HTTP Request Sampler` add a `Bean Shell Post Processor` as a child of the sampler. Add a script as below
```
vars.put("jwtToken", new String(data));
```
We can then use the variable as `${jwtToken}`

For other extraction, visit [JMeter Extract and Re-use as Variable – with More Extractors](https://www.redline13.com/blog/2018/09/jmeter-extract-and-re-use-as-variable/)

> Note: BeanShell is one of the most advanced JMeter built-in components. It supports Java syntax and extends it with scripting features like loose types, commands, and method closures. If your test case is uncommon and implementation via embedded JMeter components becomes tricky or even impossible, BeanShell can be an excellent option to achieve your objectives.

# Add JWT Authorization to HTTP Request
Article
- https://octoperf.com/blog/2018/04/23/jmeter-rest-api-testing/

Under `HTTP Request Sampler` add a `Config Element - HTTP Header Manager`. And then add header parameter with

` Name`: `Authorization`

`Value`: `Bearer ${jwtToken}`

# Show values of variables
Article
- https://stackoverflow.com/questions/3832348/jmeter-showing-the-values-of-variables

We can use `Debug Sampler`

# Multiple users with windows authentication
Article
- https://octoperf.com/blog/2017/12/14/multiple-user-login-jmeter/
- https://www.blazemeter.com/blog/how-to-test-rest-apis-with-windows-authentication-with-jmeter

Define CSV file `username_password.csv`
```
username1,password1
username2,password2
username3,password3
```

Under `Thread Group`, add `Config Element - CSV Data Set Config`
```
Filename: the csv file location
Variable Names (comma-delimited): USERNAME,PASSWORD
Sharing mode: All threads
```

Under `Test Plan` (same level as `Thread Group`), add `Config Element - HTTP Authorization Manager`
```
Username: ${USERNAME}
Password: ${PASSWORD}
Domain: <Your window domain>
Mechanism: Mostly BASIC_DIGEST
```

# Save response data into result (run JMeter in comnmand line mode)
Article
- https://stackoverflow.com/questions/46198315/how-to-save-response-data-of-a-http-sampler-in-jmeter

Add these parameters to `user.properties`
```
jmeter.save.saveservice.output_format=xml
jmeter.save.saveservice.response_data=true
jmeter.save.saveservice.samplerData=true
jmeter.save.saveservice.requestHeaders=true
jmeter.save.saveservice.url=true
jmeter.save.saveservice.responseHeaders=true
```
# Pass thread count or loop count or period via command line
Artile
- https://mkbansal.wordpress.com/2012/08/01/jmeter-command-line-script-execution-with-arguments/
- https://medium.com/@priyank.it/jmeter-pass-command-line-properties-65a431875024

The best way is to define properties within a properties file and then define user variables which will consume these properties

`mysetting.properties`
```
mysetting.maxThreads=100
mysetting.loopCount=1
mysetting.gawf.rampUp=10
```

In the command line, make sure we have option `-p mysetting.properties`

Define user variable in `User Defined Variable` config element

`Name`: `varMaxThreads`

`Value`: `${__P(mysetting.maxThreads,50)}`

And then we can use `varMaxThreads` in the Thread properties normally
