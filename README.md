# HdfsUtils
Project to help analysing HDFS metadata.

- First feature is a D3 Sunburst visualization showing HDFS space usage and/or number of files.
- Snapshot space consumption overhead analyzer (from [this discussion](https://community.hortonworks.com/questions/24063/hdfs-snapshot-space-consumption-report.html)) is coming next (stay tunned).

## Options to run
### 1- Zeppelin notebook
Just import URL below in your zeppelin instance and runs step-by-step:<br/>
https://raw.githubusercontent.com/gbraccialli/HdfsUtils/master/zeppelin/hdfs-d3.json

### [Live Preview here](https://www.zeppelinhub.com/viewer/notebooks/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2dicmFjY2lhbGxpL0hkZnNVdGlscy9tYXN0ZXIvemVwcGVsaW4vbm90ZS5qc29u)(May be removed at any time)

### 2- Build from source, running in command line and using html file
### Building
```sh
git clone https://github.com/gbraccialli/HdfsUtils.git
cd HdfsUtils
mvn clean package
````
### Basic usage
```sh
java -jar target/gbraccialli-hdfs-utils-with-dependencies.jar \
  --path=/ \
  --maxLevelThreshold=1  \
  --minSizeThreshold=-1  \
  --showFiles=false   \
  --verbose=false > ./html/out.json  
```
### Visualizing
Open html/hdfs_sunburst.html in your browser and point to  .json file you created in previous step, or copy/paste json content on right load options<br/>
<br/>
PS: note Chrome browser has security contraint that does not allow you to load local files, use one of the following options:
- Use zeppelin notebook (describe above)
- Use Safari
- Enable Chrome local files access: [instructions here](http://stackoverflow.com/questions/18586921/how-to-launch-html-using-chrome-at-allow-file-access-from-files-mode)
- Publish json in a webserver and use full URL


### Command line options:

#### --confDir=
The path to hadoop configuration directory.
Specify directory containing hadoop config files if yours are not at /etc/hadoop/conf.

#### --maxLevelThreshold=
-1 or or valid int.
Max level of directories do drill down. -1 means no limit. for example: maxLevelThreshold=3 means drill down will stop after 3 levels of subdirectories.

#### --minSizeThreshold=
-1 or valid long.
Min number of bytes in a directory to continue drill down. -1 means no limit. minSizeThreshold=1000000 means only directories greater > 1000000 bytes will be drilled down.

#### --showFiles=
true or false
Whether to show information about files. showFiles=false will show summary information about files in each directory/subdirectory.

#### --exclude=
path1,path2,...
Comma separated directories to exclude from drill down, for example: /tmp/,/user/ won't present information about those directories.

#### --doAs=
A 'do as' username (hadoop for example)
For non-kerberized cluster, you can set user to perform hdfs operations, using hdfs you won't have permissions issues. if you are using a kerberized cluster, grant read access to user performing this operation (you can use Ranger for this) 

#### --verbose=
true or false
when verbose=true, print processing details to System.err (not applied for zeppelin)

#### --path=
path to start analysis


## Special thanks to:
- [Dave Patton](https://github.com/dp1140a) who first created [HDP-Viz](https://github.com/dp1140a/HDP-Viz) where I got insipered and copied lots of code
- [Ali Bajwa](https://github.com/abajwa-hw) who created [ambari stack for Dave's project](https://github.com/abajwa-hw/hdpviz) (and helped me get it working)
- [David Streever](https://github.com/dstreev) who created (or forked) [hdfs-cli](https://github.com/dstreev/hdfs-cli), where I also copied lots of code
