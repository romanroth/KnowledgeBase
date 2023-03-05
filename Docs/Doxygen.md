---
tags:
	- Doxygen
	- C
	- C++
	- Documentation
---

# Doxygen

Doxygen can be used to generate documentation from source code.

- **Download:** [https://doxygen.nl/download.html](https://doxygen.nl/download.html) (Binary Windows installer - for Linux building from source is recommended)
- **Documentation:** [https://doxygen.nl/manual/index.html](https://doxygen.nl/manual/index.html)

On Windows the **Doxywizard** can be used to edit the Doxyfile in a visual interface.
The best way to generate a new Doxyfile is to open **Doxywizard** and configure the parameters in there, and then save it to disk.
This file can also be loaded again later. (Note: After editing the file by hand you have to "reopen" it in Doxywizard for the changes to take effect.)

To make Doxygen not look "old-fashioned" you can use **Doxygen Awesome**: [https://jothepro.github.io/doxygen-awesome-css/](https://jothepro.github.io/doxygen-awesome-css/)

Doxygen **Template** for Doxygen Awesome: [`KnowledgeBase/DoxygenTemplate/`](https://github.com/romanroth/KnowledgeBase/tree/main/DoxygenTemplate) 

In order for Doxygen to generate good documentation the code must be commented in a specific way (see: [https://doxygen.nl/manual/docblocks.html](https://doxygen.nl/manual/docblocks.html)).
For C++ for example the Javadoc style can be used (see: [https://docs.oracle.com/en/java/javase/17/docs/specs/javadoc/doc-comment-spec.html](https://docs.oracle.com/en/java/javase/17/docs/specs/javadoc/doc-comment-spec.html)).

Doxygen can also be build inside of **Jenkins** in you build pipeline.
For this Doxygen has to be installed on the node and the HTML Publisher Plugin should be installed on the Jenkins: [https://plugins.jenkins.io/htmlpublisher/](https://plugins.jenkins.io/htmlpublisher/)
In your pipeline you can then add a stage to build the documentation.

```groovy
// This assumes that the Doxyfile is in the root workspace directory.
stage('Generate Doxygen Documentation')
{
	sh('doxygen Doxyfile')
 	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'html/', reportFiles: 'index.html', reportName: 'Doxygen-Documentation', reportTitles: '', useWrapperFileDirectly: true])
}
```

The documentation will then be "hosted" on the Jenkins and can be accessed in the sidebar of the job (named after reportName in the publishHTML function).
If the documentation does not look correct, then check your Doxygen version (when using Doxygen Awesome).
It is also possible that the **CSP** policy is not set in a way that e.g. allows javascript. To fix this you can go to `<jenkins-url>/script` and paste the following into the console:
`System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "default-src * 'unsafe-inline' 'unsafe-eval';")`

> **Only do this if you know of the consequences of this action! If the server is accessible from outside the local network this could pose a serious security risk!**

## Other Links
- Doxygen with CMake: [https://vicrucann.github.io/tutorials/quick-cmake-doxygen/](https://vicrucann.github.io/tutorials/quick-cmake-doxygen/)
- [https://stackoverflow.com/questions/13368350/use-the-readme-md-file-as-main-page-in-doxygen](https://stackoverflow.com/questions/13368350/use-the-readme-md-file-as-main-page-in-doxygen)
- Build Doxygen in Jenkins: [https://plugins.jenkins.io/htmlpublisher/](https://plugins.jenkins.io/htmlpublisher/) and [https://dnaeon.github.io/building-projects-documentation-with-jenkins-and-doxygen/](https://dnaeon.github.io/building-projects-documentation-with-jenkins-and-doxygen/)
- Graphwiz for Graphs: [http://www.graphviz.org](http://www.graphviz.org)

- `doxygen -l` generates a layout file (xml) -> reference in doxyfile at `LAYOUT_FILE`
- Doxygen tag file for C++ STL: [https://stackoverflow.com/a/44743279](https://stackoverflow.com/a/44743279)
- [https://coderwall.com/p/ydwz3a/use-highlight-js-for-syntax-highlighting-in-doxygen-generated-documentation](https://coderwall.com/p/ydwz3a/use-highlight-js-for-syntax-highlighting-in-doxygen-generated-documentation)