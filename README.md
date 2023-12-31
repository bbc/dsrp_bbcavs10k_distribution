# BBCAVS:10k Dataset - readme
This is the readme file for the BBCAVS:10k dataset, published under the BBC's [Data Science Research Partnership](https://www.bbc.co.uk/rd/projects/data-science-research-partnership).

This readme may be updated from time-to-time, especially in response to questions and feedback. It is recommended that users of the dataset check this document periodically, with update details in the Git commit log.

This Readme is in three primary sections
1. **Dataset details**: Information about the dataset itself, including metadata associated with the programmes
2. **Technical details**: Technical information about the files contained within the dataset and information about the video, audio, subtitles.
3. **Legal**: Legal restrictions on dataset use, onward distribution, etc.

## Contact
The primary contact for questions regarding this dataset is andrew dot secker at bbc.co.uk

## Distribution structure
The primary location of the distribution is located on BBC R&D's Openstack infrastructure. Details regarding how to access this will have been circulated to the primary representative of each University.

    github/

        README.md (this file)
	
        checksums.sha256.txt (checksums for all files in the distribution)
	
    BBC (openstack)/

        BBCAVS10k_summary.xlsx (spreadsheet of video file IDs and metadata)
	
        filtered-video/redux/
		
            video files (.ts)
		
        subtitles/redux/
	
            subtitles files (.xml)
			Note 03/08/2023. We have been made aware some subtitles files appear to be empty. We are currently looking into the cause of this. If this may cause a problem, please contact us for an update. In the meantime, users are reminded that subtitles can be extracted from the TS file using open source tools.
		
        tsinfo/redux/
	
            files created by tsinfo (.txt)
		
        ffprobe/redux/
	
            files created by ffprobe (.json)
		
## Errors, omissions, etc.
Please report any errors found in the dataset and descriptive metadata to the BBC. In turn, we will endeavour pass corrections to the other dataset users.

# Dataset details

## General Description
This dataset is a collection of broadcast TV programmes with subtitles and associated metadata, made available by the BBC Data Science Research Partnership (DSRP) initiative for academic research use. To use this dataset you must be affiliated with a DSRP partner university and have signed and returned to the BBC a copy of a Data Usage Agreement. 

This dataset comprises of 10,160 programmes broadcast by the BBC in the UK between June 2007 and December 2021 and contains content originally recorded between 1962 and 2017. 

### Dataset Composition
Each programme is identified by a globally unique number, called a diskref, which looks like '5129483056238917697'. All programmes are contained in an MEPG transport stream (TS) file with a .ts file extension. Subtitles are provided as a supplementary XML file for convenience. A checksum for each TS file is available in this github repository to allow verification of dataset integrity.

A single spreadsheet file in .xlsx format is provided, holding descriptive metadata for each programme.

## Additional files
+ A spreadsheet of programme metadata. **This spreadsheet comprehensively describes the content of the dataset**.
+ Technical details of each audio/video file from two different commands:
    + Output from `tsinfo` (part of `tstools`)
    + Output from `ffprobe` (part of `ffmpeg`)

## Data integrity
It is most likely you are using you university's local copy of the dataset. A file containing checksums for each TS file can be found [in this repository](https://github.com/bbc/dsrp_bbcavs10k_distribution/blob/main/checksums.sha256.txt). These checksums can be used to check the integrity of *your* copy against *our* checksums.

## Metadata Spreadsheet
The distribution contains a spreadsheet containing detailed metadata about the programmes in the dataset.

Not all metadata is available for all programmes. A missing value indicates that particular attribute was not available, and should not be used to infer the presence or absence of the value in the programme itself.

A description of the columns in the spreadsheet are as follows:

**Diskref**: A unique numerical identifier for the programme.

**PIPs_title**: The title of the programme.

**PIPs_genres**: Genre(s) associated with the programme.

**PIPs_format**: Programme format.

**PIPs_ad**: The programme contains audio description.

**PIPs_aspect**: Aspect ratio as recorded (The dataset will include content recorded in 4:3 transmitted in a 16:9 frame).

**PIPs_audio**: Audio type.

**PIPs_colour**: Can be used to identify Black and White programmes.

**PIPs_original_quality**: Original recording resolution. The dataset contains programmes broadcast in HD resolution which is upscaled content originated in SD.

**PIPs_version**: Can be used to identify programmes with in-vision sign language. These will be indicated as "Signed".

**EIT_start_datetime**: The date and time of this programme recording from the programme schedule.

**EIT_duration**: The programme duration from the broadcast schedule. Note: this is the duration of the programme itself rounded to the nearest 5 minutes. This is unlikely to match with the length of the TS file itself.

**EIT_title**: The title of the programme as found in the original broadcast.

**EIT_synopsis**: The synopsis of the programme as found in the original broadcast.

**EIT_pCRID**: Programme identifier, as found in the broadcast.

**EIT_sCRID**: Series identifier, as found in the broadcast.

**ffprobe_width**: Width of the video, as reported by ffprobe. Included here for convenience, full ffprobe output for each ts file is available as part of data download.

**ffprobe_height**: Height of the video, as reported by ffprobe. Included here for convenience, full ffprobe output for each ts file is available as part of data download.

**tsinfo_video**: Video attributes as reported by tsinfo. Included here for convenience, full tsinfo output for each ts file is available as part of data download.

**First_TX**: The date and time of the first transmission of this programme.

**PIPs_synopsis_short**: A short length synopsis of the programme.

**PIPs_synopsis_mid**: A medium length synopsis of the programme. This synopsis should closely match the value of EIT_synopsis.

**PIPs_synopsis_long**: A long length synopsis of the programme.


### Genres
The dataset contains a broad variety of programme genres. Details of these genres can be found in the Metadata Spreadsheet file. Not all genres, however, are present. Certain types of programme or styles of programming have been filtered out for compliance reasons or due to editorial restrictions. A non-exhaustive list of programme types which have been explicitly excluded include:
+ News and current affairs
+ Childrens', including any content likely to contain images of children (i.e. schools programmes for primay and secondary ages)
+ Music-based programming (music programmes, documentaries about bands, albums, etc.)
+ Films
+ Sport

Within the metadata spreadsheet, a programme may be labelled with multiple genres delimited by a colon (:) character. Genres exist in a hierarchy up to 3 levels deep.

### Sign Language
Some programmes may contain a sign language interpreter in the bottom-right hand corner. The sign language used will be BSL. Users interested in sign language research are directed to the BBC/Oxford University BOBSL dataset which contains a [more comprehensive set of BSL signed content](https://www.bbc.co.uk/rd/projects/extol-dataset). Programmes with signing have been identified using **best efforts** and have been indicated in the programme metadata spreadsheet. 

### Audio Description
Some programmes will contain an audio description (AD) track in addition to the main programme audio. Programmes with AD have been identified using **best efforts** and have been indicated in the programme metadata spreadsheet. 

### Subtitles
Almost all (but not quite all) programmes include subtitles. In limited cases, subtitles may not be availabe for some live content although everything broadcast from pre-recorded material will have subtitles.

### Black and White content
The dataset contains a small number of programmes in black and white. These have been identified using **best efforts** and have been indicated in the programme metadata spreadsheet. 

# Technical details

## General
Each TS file in the dataset is an off-air recording of a broadcast (https://www.bbc.co.uk/blogs/bbcinternet/2008/10/history_of_the_bbc_redux_proje.html). Individual TS files have been created from received digital terrestrial or digital satellite transmission. As such:.
+ The PIDs on which the elementary streams are found (i.e. audio, video and subtitles) will differ between individual TS files.
+ Data other than the audio, video, subtitles will be present

Users unfamiliar with TS files may like to familiarise themselves with how elementary streams are labelled with a PID (and can therefore be extracted from the transport stream using this PID as identifier), and how elementary streams are listed in and described by the PMT. Tools such as `tsinfo` and `ffprobe` can interpret the PMT and output the contents in human readable format. Dumps of the output from `tsinfo` and `ffprobe` are provided for each TS for convenience.

The order with which elementary streams are listed in a PMT should not be assumed to be (a) consistent between TS files and (b) meaningful. Users are urged to check the attributes of the individual streams listed in the PMT to correctly identify the required PID. This may be especially important to identify the correct audio track as multiple audio tracks may be contained within a single TS file. See the section on **A/V Technical Details** for more details.

## Programme start/end alignment
The TS files have not been accurately edited around the start/end times of the primary programme. No assumptions should be made about the start or end times of the recording with regard to the start and end times of the primary content.

## A/V Technical Details

 As received from broadcasts, the TS files contain extraneous data, over and above the basic audio and video streams. In some circumstances it may be convenient/required to extract the audio and video before they can be used.

### Video resolution and frame rate
The dataset contains content in HD and SD resolutions, the highest quality version of a programme that we have access to has been included (i.e. if only an SD version is included, we cannot supply the HD version).

The dataset contains two different resolutions for HD content. The resolution of HD content broadcast by the BBC changed over the period covered by the dataset. HD content is present in 1440x1080 resolution (for older content) and 1920x1080 resolution (for newer content). The change in resolutions was due to technical limitations of how the content was broadcast in the UK and are thus represented in these off-air recordings.

The HD resolution of the video in the dataset is independent of the resolution of the original content. Some HD programmes may be upscaled content originally filmed in SD.

Due to the nature of these transport streams originating as broadcasts in the UK, there are some nuances in the video encoding which should be handled transparently by most video decoders but, if unexpected results occur, it is worth noting: 
+ The video may be encoded as a mix of interlace and progressive (50i and 25p) within the same video stream. The switch between interlace and progressive encoding will happen on a GOP boundary. (https://www.bbc.co.uk/blogs/researchanddevelopment/2011/04/software-upgrade-for-bbc-hd-on.shtml)
+ GOPs are variable length.

### Primary Audio and Audio Description
Content will contain a single primary audio track or, in the majority of examples, two audio tracks comprising of the primary audio and an audio description track. If present in the PMT, the audio description track is always present, *even if the programme does not contain any audio description*.

Be advised: Some tools used to extract the audio from a TS file may assume one audio track is present or otherwise pick the first audio track listed in the PMT as the primary audio track. This can lead to unexpected results in the output, namely the AD track (which may be silent) being output instead of the primary audio. Users may wish to check the audio language type in the PMT of the TS (using the output of tsinfo, for example) in order to identify the correct audio track, and manually force the use of the required track.

### Subtitles
All TS files contain the original subtitles streams as broadcast as DVB and/or Teletext format. Some files will contain both. If required, these can be stripped out using a number of open-source applications. The subtitling descriptor against the appropriate stream in the PMT should be checked to determine the subtitle type.

For convenience, all TS files have an accompanying XML file containing the subtitles as Timed Text format XML. All files contain subtitles as text, with associated timing data. Some files will also contain position and colour data for the subtitles. If position and/or colour data is required for the remainder, please contact us.


---

### Acknowledgements
We thank our colleagues at [BBC Archives](https://www.bbc.co.uk/archive/) for their help.
