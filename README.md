# mrx-demo

mrx-demo contains demo data for the encoding of mrx files.

## The Metadata Context

The Metarex mascot Rexy has been catching [the sun](https://metarex.media/meeja/mrx-rexy-nab-2023.mp4).
This video gave us several types of metadata that we can containerise in an mrx file,
which we can do with the
[Metarex command line tool](https://github.com/metarex-media/mrx-tool).

## Running the Demo

First install the
[Metarex command line tool](https://github.com/metarex-media/mrx-tool)
and make sure it runs on your system. This will be the tool we use to encode
and decode the metadata in an mrx file.

For FAQs about why the files are named the way they are,
or any other command line tool questions,
please see the mrx command line tool
[documentation](https://github.com/metarex-media/mrx-tool/blob/main/HELP.md).

### Encoding your first mrx file

The first set of data we'll look at encoding is the camera position and the
tail movement metadata of Rexy. This is located in the BaseMetaData folder,
there the `0000StreamTC` folder contains the frame by
frame instances of the camera metadata json files
and `0001StreamTE` contains the single embedded metadata
for the tail position CSV of Rex.

By running the following command, the metadata in the folder is enclosed in an mrx file named `first/myfirst.mrx`.
```./mrx-tool encode --input BaseMetaData/ --output first/myfirst.mrx```

The layout of the myfirst.mrx is current available in `myfirst/myfirstmrx.yaml`

### Decoding your first mrx file

Next its time to decode the mrx file, to ensure the preservation of the metadata
and to show that the metadata can be can be continually
containerised and uncontainerised in the mrx format.

Decode your mrx file back into the metadata folders by running.

```./mrx-tool decodesave --input first/myfirst.mrx --output second```

The `second` folder and its contents should now match the `BasicMetaData` folder contents.

### Adding more metadata

Now you've decoded the mrx file, we'd like to add some additional lighting metadata,
from the `AdditionalMetadata` folder. To do this, copy and paste the `0002StreamTC` folder
into the `first` folder, then add the following to the `"StreamProperties"` field in the `second/config.json`.

```json
"2": {
    "Type": "Lighting Component",
    "FrameRate": "24/1",
    "NameSpace": "https://metarex.media/reg/MRX.123.456.789.jkl"
}
```

This addition to the config json gives the namespace for the new metadata channel
and the frame rate for it to be encoded at. The config.json is wrapped and unwrapped
as part of the mrx file and is vital for the round tripping of the metadata.
The new mrx with the additional lighting data can now be encoded by running.

```./mrx-tool encode --input second/ --output second/second.mrx```

You can now extract the contents of the mrx file using the methods described earlier,
or do anything you please with the file. Please play around
and if you find any issues let us know.

## Extra Tools to Visualise MRX files

The following tools are also available to help get a greater
understanding of the contents of an MRX file. These are independently generated
mxf verification tools, showing that mrx, at its core is just an mxf file. That follows
the 20 year old standard.

- [MXF inspect](https://github.com/Myriadbits/MXFInspect) you can look at the physical layout of the file.
- [Reg-XML](https://registry.smpte-ra.org/apps/regxmldump/view/published/)
Gives more details here about the header information of an rmx file.
