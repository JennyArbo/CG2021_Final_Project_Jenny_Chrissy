# Biodiversity in US National Parks

## Coder Girl 2021 Data Science Final Project

### Chrissy Bellizzi and Jenny Arbuszewski

# Executive Summary

## Link to Presentation Slides

https://docs.google.com/presentation/d/1kMpPd5t5jq3JrvQm0gzWucCUQS5Ag9N3TZzwUHdlESQ/edit?usp=sharing 

## Background

An agency of the Department of the Interior, the US National Park Service has a history dating back to 1872, when President Grant signed an act on March 1st establishing Yellowstone as the country's first national park.  Captivated by nature since childhood, Theodore Roosevelt led several conservation initiatives during his presidency, including the 1906 Antiquities Act.  Congress offered its support in 1916 by passing the so-called Organic Act, which officially established the National Park Service.  Other significant landmarks in the NPS chronology include the Endangered Species Act of 1973, which is perhaps the best-known in a series of 1960s/70s environmental legislation that elevated the importance of research and scientific study in the national parks.

In its capacity as a conservation-focused federal agency, the National Park Service publishes and maintains publicly available data on the flora and fauna present in its 63 established parks.  This information is offered via the online tool NPSpecies, hosted on the Department of the Interior's IRMA (Integrated Resource Management Applications) portal (https://irma.nps.gov/NPSpecies/).  The dataset we utilized in this project is drawn from the NPS database and hosted on Kaggle under the name "Biodiversity in National Parks" (https://www.kaggle.com/nationalparkservice/park-biodiversity).  It comprises two subsets: parks.csv, which provides location and size information for 56 national parks; and species.csv, which offers 119,248 unique entries for plants, animals, and other organisms that reside within the bounds of those 56 national parks.

## Project Motivation

According to the National Park Service website, an estimated 80-90% of species who call our national parks "home" have yet to be discovered.  In light of factors such as climate change and pollution, the populations of these unknown species may become threatened or even extinct if no action is taken.  The data that has been collected on known park species can be used as a sample on which to base conservation efforts.  By determining which types of habitats and organisms are least common in our parks and perhaps most in danger of disappearing, conservation efforts can be objectively prioritized, allocating funding and other resources to where they are most needed.

The preservation of natural spaces and their ecosystems should not only concern folks who enjoy hiking and camping in them - their continued existence afffects all of us.  Threats to biodiversity can have ripple effect that ultimately impact humans and our livelihood.  By working to preserve these systems now, we can mitigate and potentially eliminate future risks, leading to better outcomes all around.  Responsible management of resources using an evidence-based approach, with an eye to creative and doable solutions, can preserve these resources for future generations and make measurable progress towards sustainable solutions.

## Considerations and Caveats

As mentioned in the previous section, it is thought that the majority of flora and fauna residing within park boundaries remain undiscovered, meaning our dataset is by default a sample of the actual natural population of the park system.  We discovered some additional gaps in the Kaggle dataset:
- There are currently 63 national parks in the NPS system; the dataset includes entries for 56 parks.
- National parks are found in 30 states and 2 US territories; the dataset has entries for 27 states and no territories.
- The Kaggle description includes the following: "All park species records are available to the public on the National Park Species portal; exceptions are made for sensitive, threatened, or endangered species when widespread distribution of information could pose a risk to the species in the park."  As no aggregate figures are provided (ex. a numerical count of all known species, including those whose full entries are omitted from the public-facing database), this inserts another wrinkle into how useful models built on this dataset would be.
- Our map displaying the sizes and locations of the parks represented in the dataset clearly shows that the parks are clustered on the western half of the United States, with Alaska and California leading the pack in both acreage and number of parks.  While there are certainly unique biomes and organisms in the western portion of the country that deserve attention and protection, this lopsided aspect of the dataset gives one pause as to how much models derived from it would be relevant to conservation decisions in the eastern portion of the United States.
- The National Park Service is but one of the government entities that work to preserve our natural riches for future generations.  For instance, the US Forest Service maintains more than 192 million acres of protected land than includes grasslands and environmental research areas in addition to forests.  There is certainly overlap between the national parks and forests, but comprehensive data collection, modeling, and conservation efforts should also include regionally maintained nature preserves, such as state parks, which fall outside the scope of the dataset and our project.

## Preprocessing

Multiple preprocessing approaches and techniques were utilized to clean and prepare the data for modeling:
- In examining the data, we came across a number of "misfiled" entries - ex. entries where the "Record Status" values clearly belonged to the "Common Name" field (instead of "Approved" or "In Review," one finds "Wild Iris" or "Northern Pintail").  In an ideal world, an NPS employee, intern or intrepid volunteer/Kaggle user would comb through the database to manually correct such errors.  In the interests of time, we simply dropped the affected entries, which comprise only a fraction of the species.csv rows.
- With textual values for most fields, we needed to assign numerical labels to be able to prepare the dataset for particular kinds of modeling.  Using a combination of LabelEncoder() and manual encoding based on whether a field was ordinal or nominal, we converted any alphabetic values into numeric assignments.
- NaNs were dealt with by dropping affected entries or assigning relevant labels (ex. "Not Given").
- We streamlined the dataset by dropping unnecessary or redundant columns.  Ex. While organisms' "Common Names" are useful in parlance with the general public and in dealing with wildlife on an indivdual species level, they may be less relevant to data scientists conducting macroscopic research, and even less relevant to the machine learning algorithms employed to manipulate the data.
- StandardScaler was utilized to scale the dataset prior to modeling.
- We put a fair amount of thought into how to handle the "Record Status" field.  In some ways, it seemed logical to filter the dataset to only include "Approved" records before modeling; one might reasonably argue that potential inaccurracies and inconsistencies of "In Review" records could skew the data.  At the same time, "In Review" records account for nearly 25% of the dataset, and omitting them might skew model performance as much as or more than keeping them in.  Ultimately, we chose to include both "Approved" and "In Review" records and filtering out the small quantity of entries that had some other value for "Record Status."
- Feature selection also took some deliberation.  As we derived some new columns in our initial examinations (ex. filing entries as either "Plant" or "Animal"), we exercised caution in which fields were ultimately used in building models in order to provide the algorithms with balanced data (or at least as balanced as possible, given the aforementioned considerations and caveats.)
