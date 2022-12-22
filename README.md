# LAMA Data - Capturing Entities (Persons, Topics, Music, etc.) in Austrian audio(-visual) archive material

## About the _Telling Sounds_ project and the LAMA app

The research project [_Telling Sounds_](https://mdw.ac.at/imi/tellingsounds) was concerned with the impact of the digital availability of audio(visual) sources regarding new approaches in the field of musicology.
The embedding of music in radio programs, reports, documentaries, and films points to the significance of these sources for a music history beyond the grand narrative of musical "heroes". The sources in question contain different kinds of music in a variety of contexts: the audio(visual) elements refer to different times, institutions, events, places, persons, repertoires, and sounds respectively, whose multiplicity, diversity, and spread across time and space challenge a merely linear understanding of history. Striving to link such sources across the boundaries of various collections and archives calls for a combination of methods, taken from oral history, media- and film-studies, music analysis, cultural-studies-informed-musicology and performance studies.

To meet this challenge, LAMA (Linked Annotations for Media Analysis) has been developed, a research software for capturing and visualizing the interaction of music and its contexts. LAMA aims to provide a collaborative means of building a corpus of relevant AV material, as well as adding metadata about the contents. Music, Speech, Picture, and Other Sounds appearing in a Clip can be described in detail. "Segments" can be used to add structural information about a Clip or single out sections and group annotations. Annotations can be about People, Places, Pieces of Music or historical periods but also political ideologies or _lieux de mémoire_ (i.e. sites of memory). Material mainly comes from the collections of our two principal cooperating institutions, [Mediathek Austria](https://www.mediathek.at) and [Phonogramm Archive of the Austrian Academy of Science and Research](https://www.oeaw.ac.at/phonogrammarchiv/), but could just as easily come from YouTube or other sources. A read-only version of LAMA is available [here](https://repo.mdw.ac.at/telling-sounds/lama/).

## About the data

The main concept used to enter information in the system are Annotations. They are used for capturing the various persons, topics, pieces of music and other points of interest ("entities") that appear in the Clips. Besides the relevant Entity (quotes or dates are also supported), an Annotation includes a Relation (in which way something appears in the clip; e.g. "mentioned") and timecodes.
Furthermore, Annotations can be marked as "interpretative", indicating that the annotated Entity does not appear directly but is nevertheless perceived to be present implicitly by the researcher.
Annotations can be grouped either by the sub-element of a clip (Music, Speech etc.) they're describing or by generic, user-defined Segments, which can also be used to describe the various sections of a clip.

The provided JSON file basically contains  an export of the MongoDB collections from LAMA (`annotations`, `clips`, `elements`, `entities`, `segments`).
Objects/documents are referenced by their ID, so if an Annotation references a Clip and an Entity you can find them by ID in their respective collections.

## How to cite this

This data set is published under CC0, so we waive all rights, but if you use this data set we would appreciate attribution, using the follwing citation:
Basaran et. al., *LAMA Data - Capturing Entities (Persons, Topics, Music, etc.) in Austrian audio(-visual) archive material*[data file and schemas], 2022, DOI: [10.5281/zenodo.7472509](https://zenodo.org/record/7472509).

Data Contributors: Aylin Basaran, Elias Berner, Theresa Czerny, Paul Gulewycz, Neva Klanjscek, Julia Jaklin, Birgit Michlmayr, Matej Santi, Dimitri Smirnov, Cornelia Szabo-Knotik, Meike Wilfing-Albrecht

## Schemas

### Annotation

| name | type | required | description | pattern |
| --- | --- | --- | --- | --- |
| _id | string | y | Annotation ID. | - |
| type | `Annotation` | y | Always "Annotation". | - |
| clip | string | y | ID of the Clip the Annotation belongs to. | - |
| element | string | n | ID of the associated Element, if applicable. | - |
| relation | `RBroadcastDate, RBroadcastSeries, RContributor, RDateOfCreation, RInstrument, RKeyword, RLyricsQuote, RTextQuote, RMentions, RMusicalQuote, RPerformanceDate, RPerformer, RPieceOfMusic, RProductionTechniquePicture, RProductionTechniqueSound, RQuote, RRecordingDate, RReminiscentOfMusic, RReminiscentOfNoise, RReminiscentOfPicture, RReminiscentOfSpeech, RShot, RShows, RSoundAdjective, RSoundsLike, RSoundsLikeNoise, RSpeaker, RSpeechDescriptionText` | y | The Relation expressed by the Annotation. | - |
| target | string | n | ID of the entity being annotated or of the person associated with a quote. | - |
| quotes | string | n | The text of a quote, if applicable. | - |
| date | string | n | The date being annotated, in EDTF (level 1) format. | - |
| role | string | n | ID of role-Entity (of type VClipContributorRole, VFunctionInClipRole, or VMusicPerformanceRole), if applicable. | - |
| metaDate | string | n | Supplementary date information, for example the year the annotated person was photographed or recorded, in EDTF (level 1) format. | - |
| timecodeStart | integer | n | Starting timecode of Annotation, in seconds from the beginning of the clip. | - |
| timecodeEnd | integer | n | Ending timecode of Annotation, in seconds from the beginning of the clip. | - |
| confidence | `certain, maybe, unsure` | n | Supplementary information about how confident the annotator is about the accuracy of the annotated information. | - |
| attribution | string | n | Supplementary information about the origin of the Annotation's contents, for example a bibliographic reference. | - |
| comment | string | n | Free-form comment, that will be shown in the UI. | - |
| interpretative | boolean | n | Whether the Annotation describes something which is explicitly shown or mentioned, or something that is not present explicitly, but nevertheless inferred by the annotator. | - |
| constitutedBy | array | n | IDs of other Annotations (on the same Clip) which support the interpretative Annotation. | - |
| notablyAbsent | boolean | n | Whether the Annotation is about something that would be expected to be present, but isn't. | - |
| refersTo | string | n | ID of another Annotation (on the same Clip) that the current Annotation applies to. | - |
| created | string | y | Timestamp of the Annotation's creation, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| createdBy | string | y | Username of the user who created the Annotation. | - |
| updated | string | n | Timestamp of the Annotation's last update, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| updatedBy | string | n | Username of the user who last updated the Annotation. | - |

### Clip

| name | type | required | description | pattern |
| --- | --- | --- | --- | --- |
| _id | string | y | Clip ID. | - |
| type | `Clip` | y | Always "Clip". | - |
| effectiveId | string | n | Derived ID used for deduplication purposes. | - |
| title | string | y | The Clip's title, as it appears on the source website. | - |
| label | string | n | Optional user-defined label, in case the original title is too long or unclear. | - |
| subtitle | string | n | The Clip's subtitle, if applicable. | - |
| url | string | y | The URL (preferably permalink) of the website where the Clip can be found. | - |
| platform | string | y | The ID of the platform-Entity (like "Mediathek" or "YouTube") associated with the Clip. | - |
| collections | array | n | IDs of (user-defined) collection-Entities the Clip belongs to. | - |
| fileType | `a, v` | y | Whether the Clip is an audio or video file. | - |
| duration | integer | y | Duration of the Clip in seconds. | - |
| shelfmark | string | n | Shelfmark ("Signatur") of the Clip, if applicable. | - |
| language | array | y | IDs of language-Entities that apply to the Clip. | - |
| clipType | array | y | IDs of ClipType-Entities that best describe the Clip (for example: "Interview"). | - |
| description | string | n | Free-form description, either copied from the source or user-provided. | - |
| created | string | n | Timestamp of the Clips's creation in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| createdBy | string | n | Username of the user who created the Clip in the system. | - |
| updated | string | n | Timestamp of the Clips's last update in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| updatedBy | string | n | Username of the user who last updated the Clip in the system. | - |
| updatedAny | string | n | Timestamp of last update of the Clip or any of its Annotations, in ISO8601 format. | - |
| updatedAnyBy | string | n | Username of the user who last updated the Clip or any of its Annotations. | - |

### Element

| name | type | required | description | pattern |
| --- | --- | --- | --- | --- |
| _id | string | y | The ID of the Element. | - |
| type | `Music, Noise, Picture, Speech` | y | Which aspect/layer/type of phenomenon the Element is about. | - |
| clip | string | y | ID of the Clip the Element belongs to. | - |
| label | string | y | User-provided label. | - |
| description | string | y | User-provided description explaining what the Element is referring to. | - |
| timecodes | array | y | Pairs of start and end timecodes (in seconds) identifying the relevant parts of the Clip. | - |
| created | string | y | Timestamp of the Element's creation in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| createdBy | string | y | Username of the user who created the Element in the system. | - |
| updated | string | n | Timestamp of the Element's last update in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| updatedBy | string | n | Username of the user who last updated the Element in the system. | - |

### Entity

| name | type | required | description | pattern |
| --- | --- | --- | --- | --- |
| _id | string | y | ID of the Entity. | - |
| type | `AnySound, BroadcastSeries, Broadcaster, Collection, CollectiveIdentity, CreativeWork, Event, EventSeries, FictionalCharacter, Genre, Group, LieuDeMemoire, Location, Movement, Organization, Person, PieceOfMusic, Place, Platform, ProductionTechniquePicture, ProductionTechniqueSound, Repertoire, Shot, Station, Thing, TimePeriod, Topic, Topos, VActivity, VAdjective, VClipContributorRole, VFunctionInClipRole, VClipType, VEventType, VInstrument, VLanguage, VMusicArts, VMusicPerformanceRole, VSpeechGenre` | y | The Entity's type. | - |
| label | string | y | Primary label. | - |
| description | string | y | Short description that ideally makes it clear what the Entity is referring to. | - |
| authorityURIs | array | y | Authority URIs, preferably GND, Wikidata, or MusicBrainz (for music). | - |
| analysisCategories | array | n | Analysis-categories assigned to the Entity. | - |
| attributes | object | n | Experimental attributes imported from authority data (Entity-IDs). | - |
| created | string | n | Timestamp of the Entity's creation in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| createdBy | string | n | Username of the user who created the Entity in the system. | - |
| updated | string | n | Timestamp of the Entity's last update in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| updatedBy | string | n | Username of the user who last updated the Entity in the system. | - |

### Segment

| name | type | required | description | pattern |
| --- | --- | --- | --- | --- |
| _id | string | y | Segment ID. | - |
| type | `Segment` | y | Always "Segment". | - |
| clip | string | y | ID of the Clip the Segment belongs to. | - |
| label | string | y | User-provided label. | - |
| description | string | y | User-provided description explaining what the Segment is referring to. | - |
| timecodes | array | y | Pairs of start and end timecodes (in seconds) identifying the relevant parts of the Clip. | - |
| segmentContains | array | y | IDs of Annotations belonging to the Segment, with metadata. | - |
| created | string | y | Timestamp of the Segment's creation in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| createdBy | string | y | Username of the user who created the Segment in the system. | - |
| updated | string | n | Timestamp of the Segment's last update in the system, in ISO8601 format. | `[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}(\.[0-9]{6})?\+00:00$` |
| updatedBy | string | n | Username of the user who last updated the Segment in the system. | - |
