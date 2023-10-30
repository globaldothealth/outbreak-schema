# Global.health Day Zero Outbreak schema

This is a draft schema developed by Global.health to collect information in early outbreak scenarios. An early variant of this schema was used for our [mpox linelist](https://github.com/globaldothealth/monkeypox/blob/main/data_dictionary.yml).

Machine-readable version of the schema: [outbreak.schema.json](outbreak.schema.json)

During an early outbreak scenario, it can be difficult to get detailed data. We have divided the attributes in the data dictionary by priority and category. There are two priorities, High and Medium:

* **High**: These items should be prioritised for data collection above Medium: Case_status, Location_information, Healthcare_worker (Y/N/NA), Pregnancy_Status, Vaccination, Date_report, Date_onset, Date_confirmation, Hospitalised (Y/N/NA), Date_hospitalisation, Outcome, Date_death, Contact_with_case, Source

* **Medium**: These attributes are likely to be difficult to infer from aggregate data, and hence are medium priority: Age, Sex_at_birth, Race, Ethnicity, Nationality, Occupation, Previous_infection (Y/N/NA), Vaccination (Y/N/NA), Date_of_first_consultation, Reason_for_hospitalisation, Date_discharge_hospital, Intensive_care (Y/N/NA), Isolated (Y/N/NA), Contact_setting, Transmission, Travel_history (Y/N/NA), Travel_history_location

## Schema

### Case Demographics

- **`Pathogen`** *(string)*: Constant, pathogen of interest.
- **`ID`** *(string)*: Unique string identifying the case.
- **`Case_status`**: Status of a case. Cases which are discarded were previously suspected but have now been confirmed negative, and should be excluded from case counts. Cases which are omit_error were incorrectly added and should be dismissed from any data interpretation. Must be one of: `["confirmed", "probable", "suspected", "discarded", "omit_error"]`.
- **`Pathogen_status`**: Whether the infection occured in an endemic, or non-endemic region. Must be one of: `["endemic", "emerging", "unknown"]`.
- **`Location_information`** *(object)*: (GeoJSON) Curators input an initial location where the case was reported from. This query results in a location structure returned from a geocoding API, such as Mapbox.
- **`Age`** *(string)*: Age of individual, specified as range, open-ended (<n, >n) or as a range delimited by a hyphen following 5 year increments (m-n).
- **`Sex_at_birth`**: Sex at birth of an individual. Must be one of: `["male", "female", "other"]`.
- **`Sex_at_birth_other`** *(string)*: Free text entry for the sex of an individual, if not covered by 'Sex_at_birth'.
- **`Gender`**: Gender identity of the individual. Must be one of: `["man", "woman", "transgender", "non-binary", "other"]`.
- **`Gender_other`** *(string)*: Free text entry for Gender identity if not covered by 'Gender' field.
- **`Race`** *(array)*: Race of the individual, can select multiple options.
  - **Items**: Must be one of: `["Native Hawaiian or Other Pacific Islander", "Asian", "American Indian or Alaska Native", "Black or African American", "White", "Other"]`.
- **`Race_other`** *(string)*: Free response input for the race of the individual.
- **`Ethnicity`**: Ethnicity of the individual, per the U.S. census definition. Must be one of: `["Hispanic or Latino", "Not Hispanic or Latino", "other"]`.
- **`Ethnicity_other`** *(string)*: Free response input for the ethnicity of the individual. Input text data as provided by the original data source; do not translate.
- **`Nationality`** *(array)*: Nationality of the individual, can select multiple.
- **`Nationality_other`** *(string)*: Free response entry for the nationality of the individual, if not covered by the Nationality field.
- **`Occupation`** *(string)*: Free response entry describing the individual's occupation.
- **`Healthcare_worker`**: Whether individual is a healthcare worker. Must be one of: `["Y", "N", "NA"]`.

### Medical History

- **`Previous_infection`**: Did the individual test positive for the infection prior to the most recent diagnosis. Must be one of: `["Y", "N", "NA"]`.
- **`Co_infection`** *(array)*: If the individual tested positive for another pathogen, can select multiple.
- **`Pre_existing_condition`** *(array)*: If the individual has any pre-existing conditions.
- **`Pregnancy_Status`**: Is the case pregnant or post-partum? Must be one of: `["Y", "N", "NA"]`.
- **`Vaccination`**: Has the individual received a dose of vaccine. Must be one of: `["Y", "N", "NA"]`.
- **`Vaccine_name`** *(string)*: Name of the first vaccine.
- **`Vaccination_date`** *(string, format: date)*: Date of first vaccination.
- **`Vaccine_side_effects`** *(array)*: List of symptoms experienced after receiving the vaccine.

### Clinical Presentation

- **`Symptoms`** *(array)*: List of symptoms (i.e. cough, sore throat, etc.).
- **`Date_report`** *(string, format: date)*: Date case was first reported. Suggest adding the date of first report, such as the date first included in a situation report or other official communication.
- **`Date_onset`** *(string, format: date)*: Date of onset of symptoms. May be difficult to infer from aggregate date reporting.
- **`Date_confirmation`** *(string, format: date)*: Date when case was confirmed.
- **`Confirmation_method`** *(string)*: Test used to perform diagnosis. Dependent on pathogen, and often already included in the definition.
- **`Date_of_first_consultation`** *(string, format: date)*: Date that the individual received first clinical consultation.
- **`Hospitalised`**: Whether individual was hospitalised. Must be one of: `["Y", "N", "NA"]`.
- **`Reason_for_hospitalisation`** *(array)*: Reason why the individual was hospitalised, can select multiple.
  - **Items**: Must be one of: `["monitoring", "treatment"]`.
- **`Date_hospitalisation`** *(string, format: date)*: Date individual was hospitalised.
- **`Date_discharge_hospital`** *(string, format: date)*: Date that the individual was discharged from the hospital. Note: there is a separate field for ICU discharge.
- **`Intensive_care`**: Whether individual admitted to an intensive care unit or high dependency unit at hospital. Must be one of: `["Y", "N", "NA"]`.
- **`Date_admission_ICU`** *(string, format: date)*: Date individual entered intensive care unit.
- **`Date_discharge_ICU`** *(string, format: date)*: Date that the individual was discharged from the ICU.
- **`Home_monitoring`**: Whether individual is being remotely monitored by health officials at home without hospital admission. Must be one of: `["Y", "N", "NA"]`.
- **`Isolated`**: Whether individual was isolated at home or in hospital. Dependent on context/pathogen. Must be one of: `["Y", "N", "NA"]`.
- **`Date_isolation`** *(string, format: date)*: Date individual entered isolation.
- **`Outcome`**: Outcome of the disease, recovered includes inactive case counts. Must be one of: `["recovered", "death", "ongoing post-acute condition"]`.
- **`Date_death`** *(string, format: date)*: Date individual died.
- **`Date_recovered`** *(string, format: date)*: Date individual recovered, inactive case status.

### Exposure

- **`Contact_with_case`**: Has the individual had contact with a confirmed or suspected case. Must be one of: `["Y", "N", "NA"]`.
- **`Contact_ID`** *(string)*: If specified, the ID of the confirmed or suspected contact.
- **`Contact_setting`**: Setting where contact occurred. HEALTH=healthcare, including laboratory exposure, PARTY=Sexual contact at night club/private party/sauna or similar setting, BAR=bar/restaurant/other small event where there was no sexual contact, LARGE=large event with no sexual contact (e.g. festival or sports event), LARGECONTACT=large event with sexual contact, UNK=unknown. Must be one of: `["HOUSE", "WORK", "SCHOOL", "HEALTH", "PARTY", "BAR", "LARGE", "LARGECONTACT", "OTHER", "UNK"]`.
- **`Contact_setting_other`** *(string)*: Contact setting not covered by Contact_setting.
- **`Contact_animal`**: Whether the individual has known contact with animals. PET=household pets excluding rodents, PETRODENTS=rodent pets, WILD=wild animals excluding rodents, WILDRODENTS=wild rodents. Must be one of: `["PET", "PETRODENTS", "WILD", "WILDRODENTS", "OTHER"]`.
- **`Contact_comment`** *(string)*: Free text describing any additional contact information.
- **`Transmission`**: Most likely mode of transmission. ANIMAL = Animal to human transmission. HAI = Healthcare-associated, LAB = Transmission in a laboratory due to occupational exposure, MTCT = Transmission from mother to child during pregnancy or at birth, OTHER = Other transmission, FOMITE = Contact with contaminated material (e.g bedding, clothing, objects), PTP = Person-to-person (excluding: mother-to-child, healthcare-associated or sexual transmission), SEX = Sexual transmission, TRANSFU = parenteral transmission including intravenous drug use and transfusion, UNK = Unknown. Must be one of: `["ANIMAL", "HAI", "LAB", "MTCT", "OTHER", "FOMITE", "PTP", "SEX", "TRANSFU", "UNK"]`.
- **`Travel_history`**: Whether individual has travel history, domestic and/or international. Must be one of: `["Y", "N", "NA"]`.
- **`Travel_history_entry`** *(string, format: date)*: Date when individual entered the country.
- **`Travel_history_start`** *(string, format: date)*: Date when individual began travel.
- **`Travel_history_location`** *(object)*: (GeoJSON) Location of travel obtained from geocoding API such as Mapbox.

### Laboratory Information

- **`Genomics_Metadata`** *(string)*: Which clade the viral strain belongs to.
- **`Accession_Number`** *(string)*: Accession number of the sequence uploaded to public database.

### Source Information
- **`Source`** *(string, format: uri)*: URL of news story or government source where this case was confirmed.
- **`Source_II`** *(string, format: uri)*: URL of news story or government source where this case was confirmed.
- **`Source_III`** *(string, format: uri)*: URL of news story or government source where this case was confirmed.
- **`Source_IV`** *(string, format: uri)*: URL of news story or government source where this case was confirmed.
- **`Date_entry`** *(string, format: date)*: Date case was entered into line list.
- **`Date_last_modified`** *(string, format: date)*: Last date when case was modified in line list.

