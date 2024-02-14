# Web3j-maven-plugin

Web3j maven plugin is used to create java classes based on the solidity contract files.

## Usage
The base configuration for the plugin will take the solidity files from `src/main/resources` and generates 
the java classes into the folder `src/main/java`.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.web3j</groupId>
            <artifactId>web3j-maven-plugin</artifactId>
            <version>4.8.2</version>
            <configuration>
                <soliditySourceFiles/>
            </configuration>
        </plugin>
    </plugins>
</build>
```

to run the plugin execute the goal `generate-sources`
```bash
mvn web3j:generate-sources
```


## Configuration
The are several variable to select the solidity source files, define a source destination path or change the package name.

| Name                   | Format                                                                                 | Default value                   |
| -----------------------|----------------------------------------------------------------------------------------| --------------------------------|
| `<packageName/>`       | valid java package name                                                                | `org.web3j.model`               |
| `<outputDirectory><java/></outputDirectory>` | relative or absolute path of the generated for 'Java files       | value in `<sourceDestination/>` |
| `<outputDirectory><bin/></outputDirectory>`  | relative or absolute path of the generated for 'Bin' files       | value in `<sourceDestination/>` |
| `<outputDirectory><abi/></outputDirectory>`  | relative or absolute path of the generated for 'ABI' files       | value in `<sourceDestination/>` |
| `<sourceDestination/>` | relative or absolute path of the generated files (java, bin, abi)                      | `src/main/java`                 |
| `<outputFormat/>`      | generate Java Classes(`java`), ABI(`abi`) and/or BIN (`bin`) Files (comma separated)   | `java`                          |
| `<nativeJavaType/>`    | Creates Java Native Types (instead of Solidity Types)                                  | `true`                          |
| `<soliditySourceFiles>`| Standard maven [fileset](https://maven.apache.org/shared/file-management/fileset.html) | `<soliditySourceFiles>`<br>`  <directory>src/main/resources</directory>`<br>`  <includes>`<br>`    <include>**/*.sol</include>`<br>`  </includes>`<br>`</soliditySourceFiles>`  |
| `<contract>`           | Filter (`<include>` or `<exclude>`) contracts based on the name.                       | `<contract>`<br>`  <includes>`<br>`    <include>greeter</include>`<br>`  </includes>`<br>`  <excludes>`<br>`    <exclude>mortal</exclude>`<br>`  <excludes>`<br>`</contracts>`  |
| `<pathPrefixes>`       | A list (`<pathPrefix>`) of replacements of dependency replacements inside Solidity contract.  |  |

Configuration of `outputDirectory` has priority over `sourceDestination`


## Getting Started

Create a standard java maven project. Add following `<plugin>` - configuration into the `pom.xml` file:

```xml
<plugin>
    <groupId>org.web3j</groupId>
    <artifactId>web3j-maven-plugin</artifactId>
    <version>4.5.11</version>
    <configuration>
        <packageName>com.zuehlke.blockchain.model</packageName>
        <sourceDestination>src/main/java/generated</sourceDestination>
        <nativeJavaType>true</nativeJavaType>
        <outputFormat>java,bin</outputFormat>
        <soliditySourceFiles>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.sol</include>
            </includes>
        </soliditySourceFiles>
        <outputDirectory>
            <java>src/java/generated</java>
            <bin>src/bin/generated</bin>
            <abi>src/abi/generated</abi>
        </outputDirectory>
        <contract>
            <includes>
                <include>greeter</include>
            </includes>
            <excludes>
                <exclude>mortal</exclude>
            </excludes>
        </contract>
        <pathPrefixes>
            <pathPrefix>dep=../dependencies</pathPrefix>
        </pathPrefixes>
    </configuration>
</plugin>
```

Add your solidity contract files into the folder `src/main/resources`. Make sure that the solidity files 
ends with `.sol`.


Start the generating process:


```
> mvn web3j:generate-sources

[INFO] --- web3j-maven-plugin:0.1.2:generate-sources (default-cli) @ hotel-showcase ---
[INFO] process 'HotelShowCaseProxy.sol'
[INFO] 	Built Class for contract 'HotelShowCaseProxy'
[INFO] 	Built Class for contract 'HotelShowCaseV2'
[INFO] 	Built Class for contract 'Owned'
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.681 s
[INFO] Finished at: 2017-06-13T07:07:04+02:00
[INFO] Final Memory: 14M/187M
[INFO] ------------------------------------------------------------------------

Process finished with exit code 0
```

You find the generated java classes inside the directory `src/main/java/generated/`.

Next step is to interact with the smart contract. See for that 
[deploying and interacting with smart contracts](https://web3j.readthedocs.io/en/latest/smart_contracts.html#deploying-and-interacting-with-smart-contracts) 
in the official web3j documentation.

For a multi module project configuration see following [post](https://github.com/web3j/web3j-maven-plugin/issues/14) 
from [@fcorneli](https://github.com/fcorneli). In short:  For pick up the generated java source 
files, you need the build-helper-maven-plugin configuration. Also, `${basedir}` prefix is required 
within a multi-module project.
