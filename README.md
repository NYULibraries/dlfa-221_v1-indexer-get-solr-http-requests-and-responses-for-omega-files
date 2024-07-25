# dlfa-221_v1-indexer-get-solr-http-requests-for-omega-file

See [DLFA-221: "v1 indexer: get Solr HTTP requests for Omega file"](https://jira.nyu.edu/browse/DLFA-221)

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

...and then doing manual directory and filename renames, changing "Omega-EAD" to "mos_2021".  "Omega-EAD" was used for the EAD filename, but the {{<eadid>}} is "mos_2021".

_solr-responses/_ file was created by running this command:

```shell
curl -s -o solr-responses/mos_2021.json 'http://localhost:8983/solr/findingaids/select?q=ead_ssi:mos_2021&sort=id+asc&wt=json&indent=true&rows=9999999'
```

Version information:

* Omega file: [dlts\-finding\-aids\-ead\-go\-packages/ead/testdata/omega/v0\.1\.5 /Omega\-EAD\.xml](https://github.com/NYULibraries/dlts-finding-aids-ead-go-packages/blob/7baee7dfde24a01422ec8e6470fdc8a76d84b3fb/ead/testdata/omega/v0.1.5/Omega-EAD.xml) 
