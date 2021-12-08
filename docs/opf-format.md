# OPF Format

## OpenPecha Repositories
OpenPecha etexts are stored as Git repositories. At its heart, Git is a version control system that manages and stores revisions of digital projects. OpenPecha simply uses Git to store and manage versions of Tibetan texts. Within the Git system, OPF files come with the following branches:

- Master
    - The master branch contains the .opf, which is protected
    - Only admins and text owners can directly update the master branch from the other online branches
- Publication
    - The publication branch is for collaboratively improving the text
    - Only admin or text owners can merge changes from the publication branch to the master branch
    - Github action is set up in such a way that it looks for commands issued by the owner in the commits, then updates the .opf in the master branch if there are changes in the publication branch
- Custom repos:
    - This is where users outside of the collaboration team can edit text and export the text into a desirable format—so long as the parser can handle the user’s specifications, they can collaborate with OpenPecha to create it
    - These edits are not recorded, so they are not used for updating the master branch OPF
    - The exported text is released in the “temp” section of the release
- Releases
    - Initial section contains the src text as-is; they may be plain text, ebook, HFML, word, etc
    - Temp section contains the exported text of the user outside of the collaboration
    - V### section contains the official release of the exported text

## OPF Files
OPF is an open folder format, which means it’s not a compiled file, but simply an open folder with a specific hierarchy. Every OpenPecha file consists of a base text (or base texts, in the case of works with multiple volumes) in plain text (ie, v001.txt, also called the base layer) in the “base” folder and its annotations (layer_name.yml) in the corresponding “v001” folder of the “layers” folder. OPF assumes that pecha with a single base layer has only one volume. A sample OPF file might have an internal structure something like this:

-    📁  P000780.opf
    - 📄 index.yml
    - 📄 meta.yml
    - 📁 base
        - v001.txt
        - v002.txt
    - 📁 layers
        - 📁 v001
            - 📄 Title.yml
            - 📄 Author.yml
            - 📄 Tsawa.yml
            - 📄 Yigchung.yml
        - 📁 v002
            - 📄 title.yml

In the example above, the text has the globally unique and persistent identifier “P000780”; its source text is the “base” directory. (In this case, it comes from an image scan and its raw OCR data found in the github release “v0.1”). It is then formatted as an OPF base text. This OPF has annotation layers for metadata (meta.yml), index/toc (index.yml), and titles (title.yml). “Layers” is simply a list of the annotation layers that are linked to the text, and “title” is a layer that gives formatting annotations for titles (similar to the <title></title> inline tag in HTML).

The key to the format is the Index. The Index splits a text into subsections, and gives these sections unique identifiers (UUIDs). These logical units, for convenience’s sake, use the source document’s splits. Any annotation reference is also then stored in the Index as a unique ID associated with a span of characters. Whenever there’s a change to the base text, these spans are updated. Whenever an annotation is referred to outside the Index, however, it isn’t referred to as a span (as it is in a tag system like XML, for example), but as an ID.
