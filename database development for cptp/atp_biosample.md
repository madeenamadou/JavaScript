---
title: 'Co-integrating data using JavaScript'
author: 'Youssef de Madeen Amadou'
date: '26 May 2017'
output:
 html_document:
 theme: paper
---







Following are JavaScript algorithms developed to co-integrate Biosample data for the CPTP project.



Primary source tubes pooled
---

**Variable Name  :** SAMPLE_PRIMARY_TUBE_POOLED

**Variable Label :** Indicator that the primary source tubes were pooled.

**Value Type     :** integer

**JS script**

```{javascript}
0
```



Sample collection container
---

**Variable Name  :** SAMPLE_COLLECT_CONTAINER

**Variable Label :** Container in which the sample was collected in.

**Value Type     :** integer

**JS script**

```{javascript}
$('aliquot_source').map({
  'R1': 4, //Serum tube without clot activator
  'M2': 2, //Serum separator tube with clot activator
  'L4': 3, //Potassium EDTA
  'L5': 3,
  'L6': 3,
  'UR': 7  //Polypropylene tube sterile
  },null).whenNull($('Sample_Collect_Container').trim().map({'ORAGENE SALIVA KIT':5}))

```



Sample type
---

**Variable Name  :** SAMPLE_TYPE

**Variable Label :** Indicator of the type of sample being referenced.

**Value Type     :** integer

**JS script**

```{javascript}
$('aliquot_type').map({
  'SERUM':3,
  'RBC':4,
  'PLASMA': 5,
  'BUFFY COAT':7,
  'URINE':10
  },null).whenNull($('SAMPLE_TYPE').map({'SALIVA':9}))


```



Date of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_DATE

**Variable Label :** Date on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
$('date_blood_collected').whenNull($('date_urine_collected'))
.whenNull($('SAMPLE_COLLECT_DATE')).date('yyyy-MM-dd')

```



Time of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_TIME

**Variable Label :** Time on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
//Saliva sample collection time is unknown
var t = function(x){
  return x.isNull().value() ? null : x.value() + ":00"
}.call(null,$('time_blood_collected').whenNull($('time_urine_collected')))
t
```



Date and time of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_DATETIME

**Variable Label :** The date and time on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Location of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_SITE

**Variable Label :** Location where the biological sample is collected. This corresponds to the laboratory name or site name as documented in each cohort.

**Value Type     :** text

**JS script**

```{javascript}
$('CityName').map({
  'Edmonton':'Edmonton',
  'Calgary':'Calgary',
  'East Calgary':'Calgary'
},$('LocationType').whenNull($('LocationType_April2017')).map({
1: 'Permanent Study Centre',
2: 'Mobile Study Centre'
},null))
```



Participant vital state (SOP)
---

**Variable Name  :** SAMPLE_VITAL_STATE

**Variable Label :** Documentation variable indicating the state of the participant at time of sample collection. In the CPTP context, all participants were alive at time of sample donation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
1
```



Sample collection body site (SOP)
---

**Variable Name  :** SAMPLE_COLLECT_BODY_SITE

**Variable Label :** Body site of sample collection.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
2:1,
3:1,
4:1,
7:4,
5:2},null)


```



Sample collection method (SOP)
---

**Variable Name  :** SAMPLE_COLLECT_METHOD

**Variable Label :** Collection device or parameters used during sample collection.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
2:1,
3:1,
4:1,
7:2,
5:3},null)


```



Volume collected sufficient for biological sample extraction
---

**Variable Name  :** SAMPLE_VOL_SUF_EXTRACT

**Variable Label :** Sufficient volume collected per primary tube from which the biological sample will be extracted, according to manufacturer's instructions.

**Value Type     :** integer

**JS script**

```{javascript}
//Not available
```



Saliva collection device model
---

**Variable Name  :** SAMPLE_SALIVA_COLLECT_MODEL

**Variable Label :** The model number of the saliva sample collection device.

**Value Type     :** text

**JS script**

```{javascript}
$('Sample_Collect_Container').trim().map({'ORAGENE SALIVA KIT':'OG-250'},null)

```



Saliva collection device lot
---

**Variable Name  :** SAMPLE_SALIVA_COLLECT_LOT

**Variable Label :** The lot number of the saliva sample collection device.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Time from sample collection to initial stabilization (SOP)
---

**Variable Name  :** SAMPLE_STAB_TIME

**Variable Label :** Time at which the sample is stabilized either by a preservative or gel plug in the collection tube and/or reducing the temperature to 4°C.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var mc = $this('SAMPLE_COLLECT_CONTAINER').map({
2:3, 
3:1,
4:3,
7:'-7',
5:1
},null)

var edm = $this('SAMPLE_COLLECT_CONTAINER').map({
2:5, 
3:1,
4:5,
7:'-7',
5:'-7'
},null)

$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':edm,
  'Calgary':mc,
  'Mobile Study Centre':mc
},9)


```



Temperature between biospecimen acquisition and initial stabilization (SOP)
---

**Variable Name  :** SAMPLE_PRE_STAB_TEMP

**Variable Label :** Temperature at which the sample was stored at prior to stabilization as noted in the SOP.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
mc = $this('SAMPLE_COLLECT_CONTAINER').map({
2:1, 
3:'-7',
4:1,
7:'-7',
5:'-7'
},null)

edm = $this('SAMPLE_COLLECT_CONTAINER').map({
2:2, 
3:'-7',
4:2,
7:'-7',
5:'-7'
},null)

$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':edm,
  'Calgary':mc,
  'Mobile Study Centre':mc
},9)
```



Mechanism of sample stabilization (SOP)
---

**Variable Name  :** SAMPLE_STAB_METHOD

**Variable Label :** Method used to stabilize the sample.Information derived from SOPs. 

**Value Type     :** integer

**JS script**

```{javascript}
mc = $this('SAMPLE_COLLECT_CONTAINER').map({
2:2, 
3:1,
4:3,
7:'-7',
5:1
},null)

edm = $this('SAMPLE_COLLECT_CONTAINER').map({
2:3, 
3:2,
4:3,
7:'-7',
5:1
},null)

$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':edm,
  'Calgary':mc,
  'Mobile Study Centre':mc
},9)

```



Temperature post stabilization or temporary storage temperature (SOP)
---

**Variable Name  :** SAMPLE_POST_STAB_TEMP

**Variable Label :** Temperature at which the sample was temporarily stored post stabilization.When the sample is centrifuged, it refers to the temperature at which the primary source sample is temporary stored prior first centrifugation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
mc = $this('SAMPLE_COLLECT_CONTAINER').map({
2:1, 
3:1,
4:1,
7:'-7',
5:'-7'
},null)

$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':2,
  'Calgary':mc,
  'Mobile Study Centre':mc
},3)

```



Tracking number of the shipment to off-site processing lab
---

**Variable Name  :** SAMPLE_SHIPT_TRACK_NO

**Variable Label :** Tracking number of the shipment to off-site processing lab. 

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':null,//Deep will look if he can recuparate the variable
  'Calgary':'-7',
  'Mobile Study Centre':'-7'
},9)
```



Sample shipping method to off-site processing lab (SOP)
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_METHOD

**Variable Label :** Method used to ship source samples to the processing lab.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
edm = $this('SAMPLE_COLLECT_CONTAINER').map({
2:3, 
3:3,
4:3,
7:3,
5:'1'
},null)

$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':edm,
  'Calgary':'-7',
  'Mobile Study Centre':'-7'
},9)

```



Sample transport temperature within range
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_TEMP

**Variable Label :** Indicator of whether or not temperature remained within required range during the transport from collection site to proccesing lab.The information can be issued from a data logger in the shipped box or from observation at sample arrival to processing lab (e.g. the sample is not cold).

**Value Type     :** integer

**JS script**

```{javascript}
//'Temperature went out of required range' added as category
$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':$('storagetempinrange').map({'0':2},4),//ut of range data logger but it is between 4-7; Deep will look at this.
  'Calgary':'-7',
  'Mobile Study Centre':'-7'
},null)
```



Information critical about sample transport
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_FACTOR

**Variable Label :** Factor present that may have influenced the transport of sample from collection site to processing lab.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Sample arrival date at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_DATE

**Variable Label :** Date the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
//Not transferred
```



Sample arrival time at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_TIME

**Variable Label :** Time the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
//Not transferred
```



Sample arrival date and time at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_DATETIME

**Variable Label :** Date and time the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Time to arrival at processing lab
---

**Variable Name  :** SAMPLE_COLLECT_ARRIVAL_TIME

**Variable Label :** Time between time of source sample collection and sample arrival time at processing lab.Calculated as the date and time of arrival at processing lab minus date and time of sample collection.

**Value Type     :** text

**JS script**

```{javascript}
//required variables not transferred
```



First centrifugation start time (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_START_TIME_SOP

**Variable Label :** Time between sample collection and sample first centrifugation.Information derived from SOPS.

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
2:'99', 
3:'99',
4:'99',
7:'-7',
5:'-7'
},null)
```



First centrifugation start date
---

**Variable Name  :** SAMPLE_CENTRI1_START_DATE

**Variable Label :** Date when first centrifugation was performed.Calculated from end date and time minus spin delay. 

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
7:'-7',
5:'-7'
},$('date_entered_freezer').date('yyyy-MM-dd'))
```



First centrifugation start time
---

**Variable Name  :** SAMPLE_CENTRI1_START_TIME

**Variable Label :** Time when first centrifugation was performed.Calculated from end date and time minus spin delay. 

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



First centrifugation speed (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_GFORCE

**Variable Label :** G-force speed at which first centrifugation was performed.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
mc = $('date_entered_freezer').date('yyyy-MM-dd').after(
  newValue('2010-05-13','text').date('yyyy-MM-dd')).map({
      true: 1500,
      false: 3000}).value()
edm = $('date_entered_freezer').date('yyyy-MM-dd').after(
  newValue('2010-05-13','text').date('yyyy-MM-dd')).map({
      true: 1500,
      false: 3000}).value()

$this('SAMPLE_COLLECT_CONTAINER').map({
7:'-7',
5:'-7'
},$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':edm,
  'Calgary':mc,
  'Mobile Study Centre':mc
},99999))
```



First centrifugation brake (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_BRAKE

**Variable Label :** Whether or not the centrifugation break was used.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
7:'-7',
5:'-7'
},1)
```



First centrifugation temperature (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_TEMP

**Variable Label :** Temperature at which the first centrifugation was performed.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
7:'-7',
5:'-7'
},$('date_entered_freezer').date('yyyy-MM-dd').after(newValue('2011-05-05','text').date('yyyy-MM-dd')).map({
true: 1,
false: 2})).value()

```



First centrifugation end date
---

**Variable Name  :** SAMPLE_CENTRI1_END_DATE

**Variable Label :** Date when first centrifugation ended.

**Value Type     :** text

**JS script**

```{javascript}
var t = $this('SAMPLE_INI_FREEZE_DATE')
$this('SAMPLE_COLLECT_CONTAINER').map({
2:t,
3:t,
4:t
},'-7')
```



First centrifugation end time
---

**Variable Name  :** SAMPLE_CENTRI1_END_TIME

**Variable Label :** Time when first centrifugation ended.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



First centrifugation duration (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_DURATION_SOP

**Variable Label :** Time in minutes during which sample was centrifuged.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
2:10,//'10 munites'
3:10,//'10 munites'
4:10//'10 munites'
},'-7')
```



Time to first centrifugation or the sample is processed
---

**Variable Name  :** SAMPLE_CENTRI1_PRE_DELAY

**Variable Label :** Time between sample collection and first centrifugation. Calculated as the first centrifugation start time minus the sample collection time.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Temperature after first centrifugation (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_POST_TEMP

**Variable Label :** Temperature at which the primary source sample is temporary stored at post first centrifugation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
2:1,
3:1,
4:1
},'-7')
```



Second centrifugation not applicable (SOP)
---

**Variable Name  :** SAMPLE_CENTRI2_NA

**Variable Label :** Documentation variable indicating if a second centrifugation was performed.  In the case of CPTP samples, second centrifugation did not occur.Information derived from SOPs.This variable is used to calculate SPREC code.

**Value Type     :** integer

**JS script**

```{javascript}
-7
```



Time from processing to short term storage
---

**Variable Name  :** SAMPLE_CENTR1_POST_DELAY

**Variable Label :** The time between the end of centrifugation and the time of short term storage. Calculated as sample short term storage time minus centrifugation end time.

**Value Type     :** text

**JS script**

```{javascript}
-7
```



Sample process start date
---

**Variable Name  :** SAMPLE_PROCESS_DATE

**Variable Label :** Date the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
$('date_processing_completed').date('yyyy-MM-dd')
```



Sample process start time
---

**Variable Name  :** SAMPLE_PROCESS_TIME

**Variable Label :** Time the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Sample process start date and time
---

**Variable Name  :** SAMPLE_PROCESS_DATETIME

**Variable Label :** Date and time the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Enrichment method (SOP)
---

**Variable Name  :** SAMPLE_ENRICHMENT_METHOD_SOP

**Variable Label :** Enrichment method used to obtain stored sample.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
2:1,
3:1,
4:1
},6)
```



Enrichment method
---

**Variable Name  :** SAMPLE_ENRICHMENT_METHOD

**Variable Label :** Enrichment method used to obtain stored sample.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
2:1,
3:1,
4:1
},6)
```



Impact on sample processing
---

**Variable Name  :** SAMPLE_PROCESS_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//Missing
```



Information critical to sample processing
---

**Variable Name  :** SAMPLE_PROCESS_FACTOR

**Variable Label :** Factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//Undetermined
```



Information critical to sample processing, other
---

**Variable Name  :** SAMPLE_PROCESS_NOTE

**Variable Label :** Factor that may have influenced sample processing, other.

**Value Type     :** text

**JS script**

```{javascript}
$('comments')
```



Presence of lipidemia
---

**Variable Name  :** SAMPLE_LIPIDEMIA_PRESENT

**Variable Label :** Lipidemia was observed at sample processing.It indicates whether lipids were visually present in the plasma or serum portion of the source sample.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7'},$('lipemic').map({'1':1,'0':0},null))
```



Presence of hemolysis
---

**Variable Name  :** SAMPLE_HEMOLYSIS_PRESENT

**Variable Label :** Hemolysis was observed at sample processing.It indicates whether blood was visually present in the plasma or serum portion of the source sample.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7'},$('hemolysed').map({'1':1,'0':0},null))
```



Presence of hemoturia
---

**Variable Name  :** SAMPLE_HEMOTURIA_PRESENT

**Variable Label :** Hemoturia was observed at sample processing.It indicates whether blood was visually present in the urine sample.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('hematuria').map({'1':1,'0':0},null)
$this('SAMPLE_COLLECT_CONTAINER').map({
'7':t,
'5':t
},'-7')
```



Presence of turbidity
---

**Variable Name  :** SAMPLE_TURBIDITY_PRESENT

**Variable Label :** Turbidity was observed at sample processing.It indicates whether turbidity was observed within the urine sample.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('turbid').map({'1':1,'0':0},null)
$this('SAMPLE_COLLECT_CONTAINER').map({
'7':t,
'5':t
},'-7')
```



Presence of other characteristic
---

**Variable Name  :** SAMPLE_OTHER_CHAR_PRESENT

**Variable Label :** Other characteristic was observed at sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//Undermined
```



Short Term (ST) storage time (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME_SOP

**Variable Label :** Time the sample is stored for short term.Information derived from SOPs.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Short Term (ST) storage date
---

**Variable Name  :** SAMPLE_ST_STORAGE_DATE

**Variable Label :** Date the sample is stored for short term.

**Value Type     :** text

**JS script**

```{javascript}
$('date_entered_freezer').date('yyyy-MM-dd')
```



Short Term (ST) storage time 
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME

**Variable Label :** Time the sample is stored for short term.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Short Term (ST) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TEMP_SOP

**Variable Label :** Short term (ST) temperature at which the prepared or processed sample (e.g. serum) was stored temporarily prior to long-term storage.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('date_entered_freezer').date('yyyy-MM-dd') ; 
t.after(newValue('2011-11-17','text').date('yyyy-MM-dd'))
.map({
  true:'-7',
  false: t.after(newValue('2011-06-25','text').date('yyyy-MM-dd')).map({
    true:'9',
    false:$this('SAMPLE_COLLECT_CONTAINER').map({
      2:2,
      4:2,
      3:2,
      7:2,
      5:7})
      })
      })
```



Second Short Term (ST) storage date
---

**Variable Name  :** SAMPLE_ST_STORAGE_DATE2

**Variable Label :** Date the sample is stored for second short term.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Second Short Term (ST) storage time 
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME2

**Variable Label :** Time the sample is stored for second short term.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Second Short Term (ST) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TEMP2

**Variable Label :** Second Short term (ST) temperature at which the prepared or processed sample (e.g. serum) was stored temporarily prior to long-term storage.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
//Not available
```



Sample shipping method (SOP)
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_METHOD

**Variable Label :** Method used to ship sample to the biobank (once the sample is processed). Information derived from SOPS.

**Value Type     :** integer

**JS script**

```{javascript}
$('LocationType').map({
'2' : 9,//-80freezer,
1: 3})

```



Sample shipment date to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_DATE

**Variable Label :** Date the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
//Undetermined
```



Sample shipment time to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_TIME

**Variable Label :** Time the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
//Undetermined
```



Sample shipment date and time to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_DATETIME

**Variable Label :** Date and time the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Sample arrival date at biobank storage facility
---

**Variable Name  :** SAMPLE_BIOBANK_ARRIVAL_DATE

**Variable Label :** Date the processed sample arrived at the biobank storage facility (e.g. Chicoutimi Biobank, Alberta biorepository).

**Value Type     :** text

**JS script**

```{javascript}
//Undetermined
```



Sample arrival time at biobank storage facility
---

**Variable Name  :** SAMPLE_BIOBANK_ARRIVAL_TIME

**Variable Label :** Time the processed sample arrived at the biobank storage facility (e.g. Chicoutimi Biobank, Alberta biorepository).

**Value Type     :** text

**JS script**

```{javascript}
//Undetermined
```



Sample arrival date and time at biobank storage facility
---

**Variable Name  :** SAMPLE_BIOBANK_ARRIVAL_DATETIME

**Variable Label :** Date and time the processed sample arrived at the biobank storage facility (e.g. Chicoutimi Biobank, Alberta biorepository).

**Value Type     :** text

**JS script**

```{javascript}
null
```



Sample shipping duration to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_DURATION

**Variable Label :** The calculated time between sample arrival time at biobank storage facility minus sample shipment time.

**Value Type     :** text

**JS script**

```{javascript}
//Undetermined
```



Sample transport to biobank storage facility temperature within range
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_TEMP

**Variable Label :** Indicator of whether or not temperature remained within required range during the transport from processing lab to biobank.The information can be issued from a data logger in the shipped box or from observation at sample arrival to the biobank (e.g. the sample is cold).

**Value Type     :** integer

**JS script**

```{javascript}
//Undetermined
```



Information critical about sample transport to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_FACTOR

**Variable Label :** Factor present that may have influenced the transport of sample from processing lab to biobank.

**Value Type     :** text

**JS script**

```{javascript}
//Undetermined
```



Long Term (LT) storage date
---

**Variable Name  :** SAMPLE_LT_STORAGE_DATE1

**Variable Label :** Date the sample is stored for long term.

**Value Type     :** text

**JS script**

```{javascript}
$('date_entered_freezer').date('yyyy-MM-dd')
```



Long Term (LT) storage time
---

**Variable Name  :** SAMPLE_LT_STORAGE_TIME1

**Variable Label :** Time the sample is stored for long term.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : x + ":00"
}.call(null,$('time_entered_freezer'))

t

```



Long Term (LT) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_LT_STORAGE_TEMP1

**Variable Label :** Long term (LT) temperature at which the prepared or processed sample (e.g. serum) was stored.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
5:'7'
},1)

```



Sample initial freezing date
---

**Variable Name  :** SAMPLE_INI_FREEZE_DATE

**Variable Label :** Date on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
$('date_entered_freezer').date('yyyy-MM-dd')
```



Sample initial freezing time
---

**Variable Name  :** SAMPLE_INI_FREEZE_TIME

**Variable Label :** Time on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : x + ":00"
}.call(null,$('time_entered_freezer'))

t

```



Sample initial freezing date and time
---

**Variable Name  :** SAMPLE_INI_FREEZE_DATETIME

**Variable Label :** Date and time on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Time to initial freezing
---

**Variable Name  :** SAMPLE_COLLECT_INI_FREEZE_TIME

**Variable Label :** Time between time of source sample collection (e.g. whole blood) and time the sample (e.g. serum) is first frozen for long term storage.Calculated as date and time the sample is first frozen minus date and time of sample collection.

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_INI_FREEZE_DATETIME'),$this('SAMPLE_COLLECT_DATETIME')]
.filter(function(x){return x.isNull().not().value()})
.map(function(x){return x.datetime('yyyy-MM-dd HH:mm:ss').value()})

t.length == 2 ? t.reduce(function(x,y){return x-y<0 ? null : msToTime(x-y)}) : null

function msToTime(ms) {// convert millseconds duration into HH:mm:ss.SSSS format

    if(ms==null) { return ms}
    function isneg(x) {return x < 0? "-" : '' }
    ms_abs = Math.abs(ms)
    t = ms_abs/(1000*60*60)
    hrs =  Math.trunc(t)
    min =  Math.round((t-hrs)*60)
    sec =  Math.round((((t-hrs)*60)-min)*60)
    
    hrs = hrs < 10 ?  "0"+hrs: hrs;
    min = min < 10 ?  "0"+min : min;
    sec = sec < 10 ? "0" + sec : sec;
 
    k = isneg(ms) + hrs + ":" + min + ":" + sec
    return newValue(k,'text')
}

```



Type of long term preservation agent
---

**Variable Name  :** SAMPLE_LT_STORAGE_AGENT

**Variable Label :** Type of preservative present when the sample is stored long term.

**Value Type     :** integer

**JS script**

```{javascript}
mc = $this('SAMPLE_COLLECT_CONTAINER').map({
    5:2
  },'-7')
    
$this('SAMPLE_COLLECT_SITE').map({
  'Edmonton':null,//Need clarification for edmonton location
  'Calgary':mc,
  'Mobile Study Centre':mc
},null)
```



Container Type
---

**Variable Name  :** SAMPLE_LT_STORAGE_CONTAINER

**Variable Label :** Tube in which the sample type is stored in for long term storage.

**Value Type     :** integer

**JS script**

```{javascript}
1
```



Second long term (LT) storage date
---

**Variable Name  :** SAMPLE_LT_STORAGE_DATE2

**Variable Label :** Date the sample is moved to a different long term storage conditions after being removed from its first long term storage conditions.

**Value Type     :** text

**JS script**

```{javascript}
//Missing
```



Second long term (LT) storage time
---

**Variable Name  :** SAMPLE_LT_STORAGE_TIME2

**Variable Label :** Time the sample is moved to a different long term storage conditions after being removed from its first long term storage conditions.

**Value Type     :** text

**JS script**

```{javascript}
//Missing
```



Second long term (LT) storage temperature 
---

**Variable Name  :** SAMPLE_LT_STORAGE_TEMP2

**Variable Label :** A different long term (LT) storage temperature the sample type is stored after being removed from its first long term storage conditions.

**Value Type     :** integer

**JS script**

```{javascript}
//Missing
```



Number of freeze-thaw cycles
---

**Variable Name  :** SAMPLE_FREEZE_THAW

**Variable Label :** Number of times a frozen sample has been thawed and then refrozen.

**Value Type     :** integer

**JS script**

```{javascript}
$('freezethawcycles')
```



Number of freeze-thaw cycles - theoritical
---

**Variable Name  :** SAMPLE_FREEZE_THAW_THEORITICAL

**Variable Label :** Theoritical number of times a frozen sample has been thawed and then refrozen. The information is not captured in the LIMS database but derived from another source of information.

**Value Type     :** integer

**JS script**

```{javascript}
//Missing
```



Sample thaw method (SOP)
---

**Variable Name  :** SAMPLE_THAW_TEMP

**Variable Label :** The method used, whether intentional or not, that resulted in the sample thawing as per an SOP.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('samplethawtemp').map({
  '888':-7
  },null)
```



Impact on sample storage
---

**Variable Name  :** SAMPLE_STORAGE_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample storage.

**Value Type     :** integer

**JS script**

```{javascript}
$('storagetempinrange').map({
'888':1,
0:0
})
```



Information critical about sample storage
---

**Variable Name  :** SAMPLE_STORAGE_FACTOR

**Variable Label :** Factor that may have influenced sample storage.

**Value Type     :** text

**JS script**

```{javascript}
//Undetermined
```



Sample volume measured
---

**Variable Name  :** SAMPLE_VOLUME_MEASURED

**Variable Label :** Sample volume recorded.

**Value Type     :** decimal

**JS script**

```{javascript}
$('sample_vol').map({
  1 : 1000, //ml to ul
  5 : 5000  //ml to ul
},null)
```



Sample volume theoritical
---

**Variable Name  :** SAMPLE_VOLUME_THEORITICAL

**Variable Label :** Sample volume theoritical (i.e. volume range).

**Value Type     :** decimal

**JS script**

```{javascript}
//Undetermined

```



Sample volume that has been taken off
---

**Variable Name  :** SAMPLE_VOLUME_REMOVED

**Variable Label :** The volume taken from the initial sample volume.Calculated as  the sum of the different volumes that has been taken of the initial sample volume.

**Value Type     :** decimal

**JS script**

```{javascript}
0
```



Sample volume remaining
---

**Variable Name  :** SAMPLE_VOLUME_REMAINING

**Variable Label :** Sample volume remaining after each use.Calculated as the sample volume (either measured or theoritical) minus the sample volume that has been taken off.

**Value Type     :** decimal

**JS script**

```{javascript}
$this('SAMPLE_VOLUME_MEASURED')
```



SPREC code
---

**Variable Name  :** SAMPLE_SPREC_CODE

**Variable Label :** The Sample PREanalytical code (SPREC) is a standard code used to describe the biosample preanalytical procedures.For the fluid biosample, the following information are described:-Type of sample (char 1-3)-Type of primary container (char 5-7)-Pre-centrifugation (char 9)-Centrifigation (char 11)-Second centrifugation (char 13)-Post centrifugation (char 15)-Storage condition  (char 17)Information is calculated using the associated variables.Reference: Betsou et al. 2010

**Value Type     :** text

**JS script**

```{javascript}
var t = [
  //Type of sample
  $this('SAMPLE_TYPE').map({
  1:'BLD',2:'DWB',3:'SER',4:'RBC',5:'PL1',6:'ZZZ',7:'BFF',8:'CEL',9:'SAL',10:'URN'}),
  //Type of primary container
  $this('SAMPLE_COLLECT_CONTAINER').map({
  1:'ACD',2:'SST',3:'PED',4:'CAT',5:'ORG',6:'ORG',7:'PPS'},'ZZZ','XXX'),
  //Pre-centrifugation temp & delay
  function(x,y){
      return y.map({
        '1': x.map({'<2h':'A','2-4h':'C','4-8h':'E','8-12h':'G','12-24h':'I','24-48h':'K','>48h':'M'},'Z','X').value(),
        '2':x.map({'<2h':'B','2-4h':'D','4-8h':'F','8-12h':'H','12-24h':'J','24-48h':'L','>48h':'N'},'Z','X').value(),
        '3': 'O',
        '4': 'X',
        '9': 'Z'},'X')
      }.call(null,$this('SAMPLE_CENTRI1_PRE_DELAY'),$this('SAMPLE_PRE_STAB_TEMP')),
  //Centrifugation
  function(a,b,c,d){
    var bb = b.map({'10':'10-15','15':'10-15'},b)
      return a.map({'-7':'N','9':'X','3':'Z',
      '1': bb.map({'10-15': c.group([3000,6000,10000]).map({
        '-3000': d.map({'1':'B','0':'A'},'Z'),
        '3000-6000':'E','6000-10000':'G','10000+':'I'},'Z'),
        '30': 'M'},'Z'),
      '2': bb.map({'10-15': c.group([3000,6000,10000]).map({
        '-3000': d.map({'1':'D','0':'C'},'Z'),
        '3000-6000':'F','6000-10000':'H','10000+':'J'},'Z')})
      },'X')}.call(null,$this('SAMPLE_CENTRI1_TEMP'),$this('SAMPLE_CENTRI1_DURATION_SOP'),
                  $this('SAMPLE_CENTRI1_GFORCE'),$this('SAMPLE_CENTRI1_BRAKE')),
  //Second centrifugation    
  function(a,b,c,d,e){
    var bb = b.map({'10':'10-15','15':'10-15'},b)
      return e.map({'-7':'N'},
      a.map({'-7':'N','9':'X','3':'Z',
      '1': bb.map({'10-15': c.group([3000,6000,10000]).map({
        '-3000': d.map({'1':'B','0':'A'},'Z'),
        '3000-6000':'E','6000-10000':'G','10000+':'I'},'Z')},'Z'),
      '2': bb.map({'10-15': c.group([3000,6000,10000]).map({
        '-3000': d.map({'1':'D','0':'C'},'Z'),
        '3000-6000':'F','6000-10000':'H','10000+':'J'},'Z')})
      },'X'))}.call(null,$this('SAMPLE_CENTRI1_TEMP'),$this('SAMPLE_CENTRI1_DURATION_SOP'),
                  $this('SAMPLE_CENTRI1_GFORCE'),$this('SAMPLE_CENTRI1_BRAKE'),$this('SAMPLE_CENTRI2_NA')),
                  
  //Post centrifugation  delay
  function(a,b){
      return b.map({'-7':'N','9':'X','3':'Z',
      '1': a.map({'<1h':'B','1-2h':'D','2-8h':'F','8-24h':'H','>48h':'J'},'N','X').value(),
      '2': a.map({'<1h':'A','1-2h':'C','2-8h':'E','8-24h':'G','>48h':'I'},'N','X').value()
      },'X')}.call(null,$this('SAMPLE_CENTR1_POST_DELAY'),$this('SAMPLE_CENTRI1_POST_TEMP')),
  
  //Lomg-term storage
  function(a,b){
      return a.map({'7':'Y','8':'Z',
      '1': b.map({'1':'A','2':'B','3':'V'},'Z','X').value(),
      '2': b.map({'1':'D','4':'C','5':'E'},'Z','X').value(),
      '3': b.map({'1':'G','2':'H','5':'I'},'Z','X').value(),
      '4': b.map({'1':'J','2':'K'},'Z','X').value(),
      '5': b.map({'1':'L','2':'M'},'Z','X').value(),
      '6': b.map({'1':'S','2':'T','3':'W'},'Z','X').value()
      },'X')}.call(null,$this('SAMPLE_LT_STORAGE_CONTAINER'),$this('SAMPLE_LT_STORAGE_TEMP1'))

]

t.reduce(function(x,y){return x+'-'+y})
```



Date on which the sample derivative was created.
---

**Variable Name  :** SAMPLE_DERIV_DATE

**Variable Label :** Date on which the source sample was aliquoted.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Time on which the sample derivative was created.
---

**Variable Name  :** SAMPLE_DERIV_TIME

**Variable Label :** Time on which the source sample was aliquoted.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Date and time on which the sample derivative was created.
---

**Variable Name  :** SAMPLE_DERIV_DATETIME

**Variable Label :** Date and time on which the source sample was aliquoted.

**Value Type     :** text

**JS script**

```{javascript}
null
```



Sample derivative type
---

**Variable Name  :** SAMPLE_DERIV_TYPE

**Variable Label :** Indicator of the type of sample or aliquot being referenced.

**Value Type     :** integer

**JS script**

```{javascript}
null
```



Sample derivative processing method (SOP)
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_METHOD_SOP

**Variable Label :** Processing method used to generate the sample derivative.Information derived from SOPs.

**Value Type     :** text

**JS script**

```{javascript}
null
```



DNA extraction SOP
---

**Variable Name  :** SAMPLE_DNA_EXTRACT_SOP

**Variable Label :** Name of the standard operating procedure (SOP) used to conduct DNA extration.

**Value Type     :** text

**JS script**

```{javascript}
null
```



DNA 260/280 ratio
---

**Variable Name  :** SAMPLE_DNA_260_280

**Variable Label :** DNA 260/280 ratio as obtain via a spectrophotometer.

**Value Type     :** decimal

**JS script**

```{javascript}
null
```



DNA 260/280 ratio adjusted
---

**Variable Name  :** SAMPLE_DNA_260_280_ADJ

**Variable Label :** DNA (260-320)/(280-320) ratio as obtain via a spectrophotometer.

**Value Type     :** decimal

**JS script**

```{javascript}
null
```



DNA concentration measured by picogreen
---

**Variable Name  :** SAMPLE_DNA_CONC_PICOGREEN

**Variable Label :** Concentration of DNA in ng/uL as measured by picogreen.

**Value Type     :** decimal

**JS script**

```{javascript}
null
```



DNA concentration measured by UV
---

**Variable Name  :** SAMPLE_DNA_CONC_UV

**Variable Label :** Concentration of DNA in ng/uL as measured by UV.

**Value Type     :** decimal

**JS script**

```{javascript}
null
```



b-globin PCR Score
---

**Variable Name  :** SAMPLE_DNA_PCR_BGLOBIN

**Variable Label :** Number of bands amplified during b-globin PCR analysis.

**Value Type     :** integer

**JS script**

```{javascript}
null
```



Impact on sample processing
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
null
```



Information critical to sample processing.
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_FACTOR

**Variable Label :** Factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
null
```



Information critical to sample processing, other
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_NOTE

**Variable Label :** Factor that may have influenced sample processing, other.

**Value Type     :** text

**JS script**

```{javascript}
null
```




