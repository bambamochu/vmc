
Voice model creator for CMU Sphinx
===============================================================================

This tool contains basic tools for creating a custom domain voice model for use
with the PocketSphinx decoder.  It is also possible to use the voice models 
created by this tool as the basis for a test-to-speech engine.  

Note this tool has only been tested with Linux Mint 17.3 & 18 and Ubuntu GNOME 
17.04.  No good reason it should fail elsewhere, but use at your own risk.

**Please see the LICENSE file for terms of use.**

Linux/Unix installation
-------------------------------------------------------------------------------

You should install dependencies first; this ensures that python-dev, 
PocketSphinx, etc. are available.  Second, install vmc.  Some of the packages 
need to be installed within the user's home directory; ~/tools is recommended.  
This should be specified when installing the dependencies. Full installation on 
an AMD64 computer running Mint 18 would look like this:

Commands:

    cd ~/Downloads
    git clone https://github.com/umhau/vmc.git
    cd ./vmc
    bash install.sh

If the dependencies involved were already installed, use the following to 
install only the vmc program files (old versions will be automatically 
removed).

    cd ./vmc
    sudo bash install.sh -no-deps

To remove vmc, run either of the following commands:

    vmc -remove

See use examples in the next section.

Usage Examples
-------------------------------------------------------------------------------

Add to a preexisting set of recordings, and adapt an existing acoustic model.

    vmc en-us -adapt /extant/model/location -addrecordings /audio/files/location /dictation/file/location.txt 5

Create a new model, and create a new set of audio recordings.

    vmc en-us -create /place/to/put/model -newrecordings /place/to/put/audio/files /dictation/file/location.txt 5 

Import a previously created set of recordings, and adapt a preexisting model.

    vmc en-us -adapt /extant/model/location -importrecordings /audio/files/location

File Structure
-------------------------------------------------------------------------------

Two folders are involved: the audio recordings folder and the acoustic model
folder.  These can be kept in separate places.  The acoustic model folder may 
be part of the python-pocketsphinx installation, in which case it is kept at '/usr/local/lib/python2.7/dist-packages/pocketsphinx/model/en-us'. Some files
are generated by vmc.

Note the model name is only used with files created from audio recordings. All 
the en-us files have very default names. 

Most files have default names, or are named according to the model name. File 
structure is as follows (incomplete, only showing commonly-used files):

    audio-recordings
    - [model name].fileids
    - [model name].transcription
    - mdef
    - mdef.txt
    
    acoustic-model
    - feat.params

Background
-------------------------------------------------------------------------------

This tools brings together a number of disparate data files that are needed for 
creating a voice model.  This graph illustrates the data process involved:

                   word domain
                        +
                        |
                        v
        +-------+ sentence list+----------+
        |               +                 |
        |               |                 |
        v               v                 v
    dictionary      grammar: LM    voice samples
        +               +                 +
        |               |                 |
        |               v                 |
        +--------> voice model <----------+
                    training
                        +
                        |
                        v
                   voice model

Each of these steps, starting with the sentence list (given) and ending with 
the voice model are contained within this tool.

