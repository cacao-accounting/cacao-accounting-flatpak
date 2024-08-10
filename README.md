# Flatpak packaging for Cacao Accounting Desktop

[![CI](https://github.com/cacao-accounting/cacao-accounting-flatpak/actions/workflows/flatpak.yml/badge.svg)](https://github.com/cacao-accounting/cacao-accounting-flatpak/actions/workflows/flatpak.yml)

Note: The main Cacao Accounting project is still a work in progress.

## Update requirements file

NOTE: Allways start with a clean enviroment. Match the current version of python in the freedesktop sdk, current Python is 3.11

```
rm -rf venv
python -m venv venv
venv/bin/python -m pip install -r development.txt
venv/bin/python -m pip freeze --require-virtualenv --isolated --no-input --exclude-editable --requirement requirements.txt  > requirements.txt
```

Note that cacao-accounting unit tests must past with those libraries in the virtual enviroment.

IMPORTANT: Do not run this on Windows, doing so will add unwanted libraries.

## Update python packages manifest:

Update python packages manifest with

```
venv/bin/pip install req2flatpak setuptools
venv/bin/req2flatpak --requirements-file requirements.txt --target-platforms 311-x86_64 --outfile pypi_packages.json
```

The python version must match the [python version in the freedesktop runtime](https://gitlab.com/freedesktop-sdk/freedesktop-sdk/-/blob/master/elements/components/python3.bst).

## Update the tkinter library:

The python version of python the freedesktop sdk do not come with the Tkinter module, update the Tkinter Standalone manifest with your python version:

https://github.com/iwalton3/tkinter-standalone

Current version: Python 3.11

## Create the flatpak:

```
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install flathub org.freedesktop.Platform//23.08 org.freedesktop.Sdk//23.08
flatpak run org.flatpak.Builder build-dir --force-clean com.cacaoaccounting.CacaoAccounting.yml
```

## Test the flatpak

```
flatpak-builder --user --install --force-clean build-dir com.cacaoaccounting.CacaoAccounting.yml
flatpak run com.cacaoaccounting.CacaoAccounting
```
