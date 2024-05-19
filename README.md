# Flatpak packaging of the desktop version of Cacao Accounting

Note: The main Cacao Accounting project is still a work in progress.

## Updata python packages manifest:

Update python packages manifest with

```
pip install req2flatpak setuptools
req2flatpak --requirements-file requirements.txt --target-platforms 311-x86_64 --outfile pypi_packages.json
```

The python version must match the [python version in the freedesktop runtime](https://gitlab.com/freedesktop-sdk/freedesktop-sdk/-/blob/master/elements/components/python3.bst).

## Create the flatpak:

```
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install flathub org.freedesktop.Platform//23.08 org.freedesktop.Sdk//23.08
flatpak run org.flatpak.Builder build-dir --force-clean com.cacaoaccounting.CacaoAccounting.yml
```

## Test the flatpak

$ flatpak-builder --user --install --force-clean build-dir com.cacaoaccounting.CacaoAccounting.yml
$ flatpak run com.cacaoaccounting.CacaoAccounting
