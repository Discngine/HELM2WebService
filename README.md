HELM2WebService

How to install on Tomcat version 8.0 or later versions:

1. Download HELM2WebService war file at https://oss.sonatype.org/content/repositories/releases/org/pistoiaalliance/helm/helm2-webservice/2.2.0/helm2-webservice-2.2.0.war

2. Rename the war file to WebService.war

3. Copy WebService.war file to Tomcat webapps folder (e.g. C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps) 

4. Helm Web Editor can be loaded in browser by using URL: http://localhost:8080/WebService/hwe 

5. The Swagger page for the web service can be found at http://localhost:8080/WebService/HowToUse.html


The WebService.war file is given in publish. 

To run this on tomcat or any other server, you have to add manually the ./helm repository to the server.

To call the WebService, here are some examples using java.

protected static Response validation(String notation) throws URISyntaxException {
    String extendedURL = "/Validation";
    URI uri = new URIBuilder(url + extendedURL).build();
    Client client = ClientBuilder.newClient().register(JacksonFeature.class);
    client.target(uri);
    Form f = new Form();
    f.add("HELMNotation", notation);
    return client.target(uri).request().post(Entity.entity(f, MediaType.APPLICATION_FORM_URLENCODED), Response.class);

  }

  protected static Response convertStandard(String notation) throws URISyntaxException {
    String extendedURL = "/Conversion/Canonical";
    URI uri = new URIBuilder(url + extendedURL).build();
    Client client = ClientBuilder.newClient().register(JacksonFeature.class);
    client.target(uri);
    Form f = new Form();
    f.add("HELMNotation", notation);
    return client.target(uri).request().post(Entity.entity(f, MediaType.APPLICATION_FORM_URLENCODED), Response.class);

  }

  protected static Response convertInput(String notation) throws URISyntaxException {
    String extendedURL = "/Conversion/Standard";
    URI uri = new URIBuilder(url + extendedURL).build();
    Client client = ClientBuilder.newClient().register(JacksonFeature.class);
    client.target(uri);
    Form f = new Form();
    f.add("HELMNotation", notation);
    return client.target(uri).request().post(Entity.entity(f, MediaType.APPLICATION_FORM_URLENCODED), Response.class);
  }

### To compile and debug

- install GPG from https://gnupg.org/ftp/gcrypt/binary/gnupg-w32-2.4.5_20240307.exe
- install Maven from https://maven.apache.org/download.cgi
- install Java 8 from https://www.oracle.com/java/technologies/javase-jdk8-downloads.html

generate key for GPG:

``` sh
gpg --gen-key --default-new-key-algo=rsa4096/cert,sign+rsa4096/encr
```

NB: it is important to know that the compilation and the test will use a configuration file needed by the helmtoolkit java library that is stored in :

%userprofile%/.helm

The file contains the following information:

``` sh
use.webservice=true
update.automatic=true
webservice.monomers.url=http://localhost:3001/HELM2MonomerService/rest
webservice.monomers.path=monomer/
webservice.monomers.put.path=monomer/
webservice.nucleotides.url=http://localhost:3001
webservice.nucleotides.path=path/nucleotideTemplate
webservice.nucleotides.put.path=path/nucleotideTemplate
webservice.editor.categorization.url=http://localhost:3001
webservice.editor.categorization.path=path/monomerStoreEditorCategories
use.external.monomers=false
external.monomers.path=null
use.external.nucleotides=true
external.nucleotides.path=null
use.external.attachments=false
external.attachments.path=null
```

for the compilation, it is better for you to set local usage :

``` sh
...
use.webservice=false
...
```
