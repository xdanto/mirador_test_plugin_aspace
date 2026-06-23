# Mirador Viewer for ArchivesSpace

Supports IIIF manifests from ContentDM in SUI and allows for mirador viewer integration in PUI.

Compatible with ArchivesSpace 4.2

Tested with ArchivesSpace 4.2

**Credit:** Adapted from a plugin built by Kevin Clair, Eberly Family Special Collections Library. Removed Kaltura for Cranbrook.

## In ArchivesSpace config.rb
Change the following line:

AppConfig[:plugins] = ['local', 'lcnaf', 'mirador']

## Getting the manifest from ContentDM
The relationship between the ContentDM URI and ContentDM's IIIF URI can be best expressed per below:

  **Example Original Link:** https://example.oclc.org/digital/collection/p16296coll13/id/16299/rec/3

  **Example IIIF Manifest:** https://example.oclc.org/iiif/2/p16296coll13:16299/manifest.json

These two links express the same information, though the later is expressed in JSON, which will be parsed with Mirador to embed neatly within ArchivesSpace. The way to make ANY ContentDM link into a IIIF link is to follow this pattern: 
  replace '/digital/collection/' with '/iiif/2/'; 
  then remove the '/id/' and replace with a colon (':');
  disregard any information from '/rec/' onwards;
  add 'manifest.json' at the end.

Although it sounds a bit verbose, this can be adapted into an excel formula. If you export a TSV from ContentDM, use a formula like this to get the IIIF manifest from the original link:
```
=SUBSTITUTE(
  "https://example.oclc.org/iiif/2/" &
  MID(V343, FIND("/digital/collection/", V343)+12, FIND("/id/", V343)-FIND("/digital/collection/", V343)-12) &
  ":" &
  MID(V343, FIND("/id/", V343)+4, LEN(V343)) &
  "/manifest.json",
  " ", ""
)
```

## In ArchivesSpace SUI
For digital objects, adjust the following variables:

  **File URI:** [ContentDM IIIF link goes here]
  
  **XLink Actuate Attribute:** onLoad
  
  **XLink Show Attribute:** embed
