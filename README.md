# mrx-demo

welcome to the demo for the [Metarex command line tool](https://github.com/metarex-media/mrx-tool)

This demo is for interacting with [metarex](https://metarex.media/) mrx files,
using the aforementioned command line tool.

The demo walks you through:

- encoding metadata files into an mrx file
- decoding an mrx file into the metadata files
- extracting the structure of an mrx file
- round tripping mrx files to and from the metadata files

## The Metadata Context

The Metarex mascot Rexy has been generating metadata
while catching [the sun](https://metarex.media/meeja/mrx-rexy-nab-2023.mp4).
In the following demo we will containerise all
the different types of metadata from this video
into a single mrx file.
This can be done with the
[Metarex command line tool](https://github.com/metarex-media/mrx-tool).

## Running the Demo

First install the
[Metarex command line tool](https://github.com/metarex-media/mrx-tool)
and make sure it runs on your system. This will be the tool we use to encode
and decode the metadata in an mrx file.

For FAQs about why the file naming system,
or any other command line tool questions,
please see the mrx command line tool
[documentation](https://github.com/metarex-media/mrx-tool/blob/main/HELP.md).

### Encoding your first mrx file

The first set of data we'll look at encoding is the camera position and the
tail movement metadata of Rexy. This is located in the BaseMetaData folder,
there the `0000StreamTC` folder contains the frame by
frame instances of the camera metadata json files
and the `0001StreamTE` folder contains the single embedded metadata
for the tail position CSV of Rexy.

By running the following command, the metadata in the `BaseMetaData`
folder will be enclosed in an mrx file named `first/myfirst.mrx`.

```cmd
./mrx-tool encode --input BaseMetaData/ --output first/myfirst.mrx
```

The layout of the myfirst.mrx can be decoded with the
following command. This will make the `myfirst/myfirstmrx.yaml` file

```cmd
./mrx-tool decode --input first/myfirst.mrx --output myfirst/myfirstmrx.yaml
```

Checkout the structure file `myfirst/myfirstmrx.yaml`, see how it has three body partitions,
one for the clocked data, one for the embedded data and one for the
mrx manifest file.

### Decoding your first mrx file

Next its time to decode the mrx file, to ensure the preservation of the metadata
and to show that the metadata can be can be continually
containerised and uncontainerised in the mrx format.

Decode your mrx file back into the metadata files by running,
by exporting them to the `second` folder.

```cmd
./mrx-tool decodesave --input first/myfirst.mrx --output second
```

The `second` folder and its contents should now match the `BasicMetaData` folder contents.
Checkout the `second/config.json` manifest file, seen how it now has information
about all the metadata files, compared to the `BaseMetaData/config.json`
manifest file.

### Adding more metadata

Now you've decoded the mrx file, we'd like to add some additional lighting metadata,
from the `AdditionalMetadata` folder. To do this, copy and paste the `0002StreamTC` folder
into the `second` folder, then add the following to the `"StreamProperties"` field in the `second/config.json`.

```json
"2": {
    "Type": "Lighting Component",
    "FrameRate": "24/1",
    "NameSpace": "https://metarex.media/ui/reg/MRX.123.456.789.jkl"
}
```

This addition to the config json gives the namespace for the new metadata channel
and the frame rate for it to be encoded at. The names space helps clarify and define
the metadata. The config.json is wrapped and unwrapped
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
Gives more details here about the header information of an mrx file.
