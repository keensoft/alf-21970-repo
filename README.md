
Alfresco Patch to High CPU consumption when parsing PDF files
================================================

Described at issue ![ALF-21970](https://issues.alfresco.com/jira/browse/ALF-21970)

PDF content extractors causing high CPU/memory with some PDFs.

Certain PDFs can cause OOM on the system when content extraction is run on them. It's been found that heap usage can easily be exhausted when attempting to extract the content when several of these files are uploaded at once. Uploading 10 of these files at once will likely cause an DoS.

When affected by this issue, following thread dump will be observed:

```
java.lang.Thread.State: RUNNABLE
 at java.lang.Object.hashCode(Native Method)
 at java.util.HashMap.hash(HashMap.java:338)
 at java.util.HashMap.put(HashMap.java:611)
 at org.apache.pdfbox.pdmodel.PDResources.reverseMap(PDResources.java:658)
 at org.apache.pdfbox.pdmodel.PDResources.setXObjects(PDResources.java:332)
 at org.apache.pdfbox.pdmodel.PDResources.getXObjects(PDResources.java:269)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:286)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.extractImages(PDF2XHTML.java:288)
 at org.apache.tika.parser.pdf.PDF2XHTML.endPage(PDF2XHTML.java:220)
 at org.apache.pdfbox.util.PDFTextStripper.processPage(PDFTextStripper.java:460)
 at org.apache.pdfbox.util.PDFTextStripper.processPages(PDFTextStripper.java:383)
 at org.apache.pdfbox.util.PDFTextStripper.writeText(PDFTextStripper.java:342)
 at org.apache.tika.parser.pdf.PDF2XHTML.process(PDF2XHTML.java:117)
 at org.apache.tika.parser.pdf.PDFParser.parse(PDFParser.java:150)
 at org.alfresco.repo.content.transform.TikaPoweredContentTransformer.transformInternal(TikaPoweredContentTransformer.java:244)
 at org.alfresco.repo.content.transform.AbstractContentTransformer2.transform(AbstractContentTransformer2.java:250)
 at org.alfresco.repo.content.transform.AbstractContentTransformer2.transform(AbstractContentTransformer2.java:202)
 at org.alfresco.repo.web.scripts.solr.NodeContentGet.execute(NodeContentGet.java:206)
 ```

Sample PDF to produce this scenario is provided at [Tika-Issue-Full-CPU.PDF](https://github.com/keensoft/alf-21970-repo)

**License**
The patch is licensed under the [LGPL v3.0](http://www.gnu.org/licenses/lgpl-3.0.html). 

**State**
Current patch release is 1.0.0

**Compatibility** 
The current version has been developed using Alfresco 201605 and Maven. It's also relevant for Alfresco 201707.

Downloading the ready-to-deploy-JAR
--------------------------------------
The binary distribution is made of one JAR file to be deployed in Alfresco as an **endorsed lib**:

* [JAR](https://github.com/keensoft/alf-21970-repo/releases/download/1.0.0/alf-21970-repo-1.0.0.jar)

You can install it by copying JAR file to `$ALFRESCO_HOME/tomcat/endorsed` and re-starting Alfresco.


Building the artifacts
----------------------
You can build the artifacts from source code using maven
```$ mvn clean package```

