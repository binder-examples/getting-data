# How to get data into your Binder

[![Binder](http://mybinder.org/badge_logo.svg)](http://mybinder.org/v2/gh/binder-examples/getting-data/master?filepath=Sentinel2.ipynb)

This example demonstrates a few ways to get data into your binder.

## Small public data

The simplest approach for small data files that are public is to add them directly to your GitHub repository. This way they are directly baked into the environment and versioned together with your code.

Works well for files with sizes up to maybe 10MB.

An example of this is `data/gapminder_all.csv`


## Medium public files

For medium sized files, a few 10s of megabytes to a few hundred megabytes, you can add a special file named `postBuild` to your repository. This will let you fetch the data when the container is built. It increases the image size but means users don't have to download the dataset each time they start the binder. And you know it will always be the same data, even if the source becomes unavailable.

More details on [the `postBuild` file](http://repo2docker.readthedocs.io/en/latest/config_files.html#postbuild).

### How to do it
Go to your GitHub repository and create a file called `postBuild`. In your
`postBuild` add the following line:

```
wget -q -O bikes-2016.csv "https://data.stadt-zuerich.ch/dataset/verkehrszaehlungen_werte_fussgaenger_velo/resource/ed354dde-c0f9-43b3-b05b-08c5f4c3f65a/download/2016verkehrszaehlungenwertefussgaengervelo.csv"
```

This will download a dataset measuring cycling and walking activity in the city of Zurich
in the year 2016.

Other methods for fetching data files will also work. We used `wget` because it
is a well known tool, no other reason.


## Large public files

For large files it is not practical to place them in your GitHub repository nor to include them directly in the container image.

> Note: We can't use technical measures to stop you from including very large files in your image. However large images take longer to launch, as well as taking up storage space that mybinder.org has to pay for. Please be considerate.

The best option for large files is to use a library specific to the data format to stream the data as you are using it. An alternative is to download each file on demand as part of your code, this way we only create network traffic when it is really needed.

There are a few restrictions on outgoing traffic from your Binder that are imposed by the team operating https://mybinder.org. Currently only connections to HTTP and Git are allowed. This comes up when people want to use FTP sites to fetch data. For security reasons FTP will never be allowed on https://mybinder.org.

> Note: to start a discussion of opening additional ports create a new issue on the [mybinder.org repository](https://github.com/jupyterhub/mybinder.org-deploy/).

### How to do it

This really depends on your data format and libraries that support accessing it
over a network. An example of accessing Sentinel 2 images that are several gigabyte
in size is in [`Sentinel2.ipynb`](Sentinel2.ipynb).


## Private files

There currently is no way to access files which are not public from https://mybinder.org.

For security reasons you should consider all information in a Binder as public.
This means:
* there should be no secrets (passwords, tokens, keys, etc) in your
GitHub repository
* you should not type passwords into a running Binder on mybinder.org
* you should not upload your private SSH key or API token to a running Binder

To support access to private files you will have to create a local deployment
of [BinderHub](https://binderhub.readthedocs.io/) where you can then decide
on the security trade offs yourself.


# Contributing and other examples

If you know of other examples for the large files section please contribute them
to this repository. If you find a mistake in this repository feel free to open
an issue or directly contribute a fix.
