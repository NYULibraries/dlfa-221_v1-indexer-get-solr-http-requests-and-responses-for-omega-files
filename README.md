# dlfa-221_v1-indexer-get-solr-http-requests-for-omega-file

See [DLFA-221: "v1 indexer: get Solr HTTP requests for Omega files"](https://jira.nyu.edu/browse/DLFA-221)

_prettified-xml/_ was created by running these commands at the root of the repo:

```shell
export TARGET_DIR=$PWD/prettified-xml
for f in $( find http-requests -type f )
do 
        repository=$( basename $( dirname $( dirname $f ) ) )
        eadid=$( basename $( dirname $f ) )
        filename=$( basename $f | sed 's/\.txt$/.xml/' )
        targetDirectory=$TARGET_DIR/$repository/$eadid/
        mkdir -p $targetDirectory
        targetFile=$TARGET_DIR/$repository/$eadid/$filename
        perl -0pe 's/^POST.*\r\n\r\n//ms' $f | xmllint --format - > $targetFile
done
```

Note that the first time this was done for the DLTS Omega file, had to do manual directory and filename renames, changing "Omega-EAD" to "mos_2021" in _dlts/_.  "Omega-EAD" was used for the EAD filename, but the {{<eadid>}} is "mos_2021".
The initial newer _appdev/_ file was based on the renamed _dlts/_.

_solr-responses/_ files were created by running this command (DEPARTMENT = dlts or appdev):

```shell
curl -s -o solr-responses/[DEPARTMENT]/mos_2021.json 'http://localhost:8983/solr/findingaids/select?q=ead_ssi:mos_2021&sort=id+asc&wt=json&indent=true&rows=9999999'
```

...right after either the _dlts/_ or the _appdev/_ file was indexed.


Version information:

* DLTS Omega file: [dlts\-finding\-aids\-ead\-go\-packages/ead/testdata/omega/v0\.1\.5/Omega\-EAD\.xml](https://github.com/NYULibraries/dlts-finding-aids-ead-go-packages/blob/7baee7dfde24a01422ec8e6470fdc8a76d84b3fb/ead/testdata/omega/v0.1.5/Omega-EAD.xml)
* AppDev Omega file: [go\-ead\-indexer/pkg/ead/testutils/testdata/fixtures/ead\-files /mos\_2021\.xml](https://github.com/NYULibraries/go-ead-indexer/blob/34a793e191c708103a95798fd970dbf501294f09/pkg/ead/testutils/testdata/fixtures/ead-files/mos_2021.xml)
