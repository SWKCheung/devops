# XWIKI Readme.md

Direct all browser insecure HTTP to HTTPs traffics.

## Installation

Generate keyfiles with certbot by the following command in Windows client
```cmd
certbot certonly --standalone

domain=>  bot.cloudority.com

Keys should be generated as follow:

C:\Certbot\live\bot.cloudority.com\fullchain.pem
Your key file has been saved at:
C:\Certbot\live\bot.cloudority.com\privkey.pem

```

Open server.xml typically found in tomcat/conf and change:


```xml
Change the redirectPort 8443 -> 443 

    <Connector port="8080" protocol="HTTP/1.1" URIEncoding="UTF-8"
               connectionTimeout="20000" acceptCount="100" enableLookups="false" maxThreads="150" 
               redirectPort="443" />
    :
    :
    :
    <Connector port="443" protocol="org.apache.coyote.http11.Http11AprProtocol"
               maxThreads="150" SSLEnabled="true" >
        <UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
        <SSLHostConfig>
            <Certificate certificateKeyFile="ssl/privkey.pem"
                         certificateFile="ssl/fullchain.pem"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>

```

In addition, add the following lines to the bottom of web.xml file right before the web-app tag
```xml 

	<!-- Force HTTPS, required for HTTP redirect! -->
	<security-constraint>
		<web-resource-collection>
			<web-resource-name>Entire Application</web-resource-name>
				<url-pattern>/*</url-pattern>
		</web-resource-collection>
	 
	<!-- auth-constraint goes here if you require authentication -->
		<user-data-constraint>
			<transport-guarantee>CONFIDENTIAL</transport-guarantee>
		</user-data-constraint>
	</security-constraint>


```




## Usage

```python
docker stact deploy -c docker-compose.yml xwiki

gocecace56fx   xwiki_postgres-xwiki   replicated   1/1        postgres:v1   *:5432->5432/tcp
7z0r0a2a8pqi   xwiki_xwiki            replicated   1/1        xwiki:v1      *:80->8080/tcp, *:443->443/tcp


```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)