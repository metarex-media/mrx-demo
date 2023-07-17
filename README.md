# mrx-demo
demo data for metarex

The metarex mascot has been catching [the sun](https://metarex.media/meeja/mrx-rexy-nab-2023.mp4). We have the metadata
from the construction of the video. And now we are going to group them together
and edit them using 
[the metarex command linetool.](https://github.com/metarex-media/mrx-tool)

## Running the demo

First install 
[the metarex command linetool](https://github.com/metarex-media/mrx-tool) and make
sure it runs on your system. This will be the tool used to encode the metadata.

Nomenclature questions go here - the help.md of the metarex tool [help.md]

### Encoding your first mrx file

The first set of data we'll look at encoding is the camera position and the tail movement metadata of Rexy. 
This is located in the BaseMetaData folder, there the `0000StreamTC` folder contains the frame by frame instances of the camera metadata
and `0001StreamTE` contains the embedded metadata for the tail position of Rex.

By running the following command, the metadata in the folder is enclosed in an mrx file named `Generated/myfirst.mrx`.
```./mrx-tool encode --input BaseMetaData/ --output Generated/myfirst.mrx```


### Decoding your mrx file

Next its time to decode the mrx file, to ensure the preservation of the metadata and to show that the metadata can be roundtripped, which
 is the contaierising and uncontaiereisng metada.

Decode your mrx file back into it's metadata by running.

 ```./mrx-tool decodesave --input Generated/myfirst.mrx --output Extracted```

 The Extracted folder and its contents should now match the BasicMetaData folder.


### Adding more metadata



Now you've decoded the mrc file, we'd like to add some additional lighting metadata, 
from the `AdditionalMetadata` folder. To do this, copy and paste the `0002StreamTC` folder 
into the extracted folder, then add the following to the `"StreamProperties"` field to the Extracted/config.json. 

```json
"2": {
    "Type": "Lighting Component",
    "FrameRate": "24/1",
    "NameSpace": "https://metarex.media/reg/MRX.123.456.789.001"
}
```

This addition to the config json gives the namespace for the new metadata 
and the frame rate for it to be encoded at. 
The new mrx with the additional lighting data can now be encoded by running.
```./mrx-tool encode --input BaseMetaData/ --output Generated/myfirst.mrx```




## Extra Tools to interperet MRX files

Using MXF inspect you can look at the layout of the file.
Reg-XML also gives more details here about the header ifnformation.
