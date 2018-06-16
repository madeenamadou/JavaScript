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
$('PRIMARY_TUBE_POOLED')
```



Sample collection container
---

**Variable Name  :** SAMPLE_COLLECT_CONTAINER

**Variable Label :** Container in which the sample was collected in.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_COLLECT_CONTAINER')

```



Sample type
---

**Variable Name  :** SAMPLE_TYPE

**Variable Label :** Indicator of the type of sample being referenced.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_TYPE')
```



Date of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_DATE

**Variable Label :** Date on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_COLLECT_DATE').date('yyyy-MM-dd')
```



Time of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_TIME

**Variable Label :** Time on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
}.call(null,$('SAMPLE_COLLECT_TIME').trim())

t
```



Date and time of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_DATETIME

**Variable Label :** The date and time on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_COLLECT_DATE'),$this('SAMPLE_COLLECT_TIME')]
.filter(function(x){return x.isNull().not().value()})

t.length == 2 ? t.reduce(function(x,y){return x.concat(' ',y)
                                               .datetime('yyyy-MM-dd HH:mm:ss')
                                               .format('yyyy-MM-dd HH:mm:ss')
                                               }) : null
```



Location of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_SITE

**Variable Label :** Location where the biological sample is collected. This corresponds to the laboratory name or site name as documented in each cohort.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_COLLECT_SITE')
```



Participant vital state (SOP)
---

**Variable Name  :** SAMPLE_VITAL_STATE

**Variable Label :** Documentation variable indicating the state of the participant at time of sample collection. In the CPTP context, all participants were alive at time of sample donation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('VITAL_STATE')
```



Sample collection body site (SOP)
---

**Variable Name  :** SAMPLE_COLLECT_BODY_SITE

**Variable Label :** Body site of sample collection.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_COLLECT_BODY_SITE')
```



Sample collection method (SOP)
---

**Variable Name  :** SAMPLE_COLLECT_METHOD

**Variable Label :** Collection device or parameters used during sample collection.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_COLLECT_METHOD')
```



Volume collected sufficient for biological sample extraction
---

**Variable Name  :** SAMPLE_VOL_SUF_EXTRACT

**Variable Label :** Sufficient volume collected per primary tube from which the biological sample will be extracted, according to manufacturer's instructions.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_VOL_SUF_EXTRACT')
```



Saliva collection device model
---

**Variable Name  :** SAMPLE_SALIVA_COLLECT_MODEL

**Variable Label :** The model number of the saliva sample collection device.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SALIVA_COLLECT_DEVICE_MODEL')

$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':t,
  '6':t},'-7')
```



Saliva collection device lot
---

**Variable Name  :** SAMPLE_SALIVA_COLLECT_LOT

**Variable Label :** The lot number of the saliva sample collection device.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SALIVA_COLLECT_DEVICE_LOT')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':t,
  '6':t},'-7')
```



Time from sample collection to initial stabilization (SOP)
---

**Variable Name  :** SAMPLE_STAB_TIME

**Variable Label :** Time at which the sample is stabilized either by a preservative or gel plug in the collection tube and/or reducing the temperature to 4°C.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_STAB_TIME')
```



Temperature between biospecimen acquisition and initial stabilization (SOP)
---

**Variable Name  :** SAMPLE_PRE_STAB_TEMP

**Variable Label :** Temperature at which the sample was stored at prior to stabilization as noted in the SOP.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_PRE_STAB_TEMP')
```



Mechanism of sample stabilization (SOP)
---

**Variable Name  :** SAMPLE_STAB_METHOD

**Variable Label :** Method used to stabilize the sample.Information derived from SOPs. 

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_STAB_METHOD')
```



Temperature post stabilization or temporary storage temperature (SOP)
---

**Variable Name  :** SAMPLE_POST_STAB_TEMP

**Variable Label :** Temperature at which the sample was temporarily stored post stabilization.When the sample is centrifuged, it refers to the temperature at which the primary source sample is temporary stored prior first centrifugation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_POST_STAB_TEMP')
```



Tracking number of the shipment to off-site processing lab
---

**Variable Name  :** SAMPLE_SHIPT_TRACK_NO

**Variable Label :** Tracking number of the shipment to off-site processing lab. 

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_SHIPMENT_TRACKING_NUMBER')
```



Sample shipping method to off-site processing lab (SOP)
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_METHOD

**Variable Label :** Method used to ship source samples to the processing lab.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SHIPMENT_TO_PROCESS_LAB_METHOD')
```



Sample transport temperature within range
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_TEMP

**Variable Label :** Indicator of whether or not temperature remained within required range during the transport from collection site to proccesing lab.The information can be issued from a data logger in the shipped box or from observation at sample arrival to processing lab (e.g. the sample is not cold).

**Value Type     :** integer

**JS script**

```{javascript}
$('SHIPMENT_TO_PROCESS_LAB_TP_RANGE')
```



Information critical about sample transport
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_FACTOR

**Variable Label :** Factor present that may have influenced the transport of sample from collection site to processing lab.

**Value Type     :** text

**JS script**

```{javascript}
$('SHIPMENT_TO_PROCESS_LAB_FACTOR')
```



Sample arrival date at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_DATE

**Variable Label :** Date the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_ARRIVAL_DATE').date('yyyy-MM-dd')

```



Sample arrival time at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_TIME

**Variable Label :** Time the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
  }.call(null,$('SAMPLE_ARRIVAL_TIME').trim())
t
```



Sample arrival date and time at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_DATETIME

**Variable Label :** Date and time the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_ARRIVAL_DATE'),$this('SAMPLE_ARRIVAL_TIME')]
.filter(function(x){return x.isNull().not().value()})

t.length == 2 ? t.reduce(function(x,y){return x.concat(' ',y)
                                               .datetime('yyyy-MM-dd HH:mm:ss')
                                               .format('yyyy-MM-dd HH:mm:ss')
                                               }) : null
```



Time to arrival at processing lab
---

**Variable Name  :** SAMPLE_COLLECT_ARRIVAL_TIME

**Variable Label :** Time between time of source sample collection and sample arrival time at processing lab.Calculated as the date and time of arrival at processing lab minus date and time of sample collection.

**Value Type     :** text

**JS script**

```{javascript}
[$('SAMPLE_COLLECT_TO_ARRIVAL_TIME')].map(function(x){
  return x.isNull().value() ? null : msToTime(x * 3600000)
  }).pop()
  
function msToTime(ms) {// convert millseconds duration into HH:mm:ss.SSSS format

    if(ms==null) { return ms}
    function isneg(x) {return x < 0? "-" : '' }
    ms_abs = Math.abs(ms)
    t = ms_abs/(1000*60*60)
    hrs =  Math.trunc(t)
    min =  Math.trunc((t-hrs)*60)
    sec =  Math.round((((t-hrs)*60)-min)*60)

    hrs = hrs < 10 ?  "0" + hrs : hrs;
    min = min < 10 ?  "0" + min : min;
    sec = sec < 10 ?  "0" + sec : sec;
    
    k = isneg(ms) + hrs + ":" + min + ":" + sec
    return newValue(k,'text')
}
```



First centrifugation start time (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_START_TIME_SOP

**Variable Label :** Time between sample collection and sample first centrifugation.Information derived from SOPS.

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},'99')
```



First centrifugation start date
---

**Variable Name  :** SAMPLE_CENTRI1_START_DATE

**Variable Label :** Date when first centrifugation was performed.Calculated from end date and time minus spin delay. 

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},$('CENTRI1_START_DATE').date('yyyy-MM-dd'))
```



First centrifugation start time
---

**Variable Name  :** SAMPLE_CENTRI1_START_TIME

**Variable Label :** Time when first centrifugation was performed.Calculated from end date and time minus spin delay. 

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},$('CENTRI1_START_TIME'))
```



First centrifugation speed (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_GFORCE

**Variable Label :** G-force speed at which first centrifugation was performed.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},$('CENTRI1_GFORCE'))
```



First centrifugation brake (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_BRAKE

**Variable Label :** Whether or not the centrifugation break was used.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},$('CENTRI1_BRAKE'))
```



First centrifugation temperature (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_TEMP

**Variable Label :** Temperature at which the first centrifugation was performed.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},$('CENTRI1_TEMP'))
```



First centrifugation end date
---

**Variable Name  :** SAMPLE_CENTRI1_END_DATE

**Variable Label :** Date when first centrifugation ended.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('CENTRI1_END_DATE')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},t.map({'NA':null},t.date('yyyy-MM-dd')))
```



First centrifugation end time
---

**Variable Name  :** SAMPLE_CENTRI1_END_TIME

**Variable Label :** Time when first centrifugation ended.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('CENTRI1_END_TIME')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},t.map({'NA':null},t.value()))
```



First centrifugation duration (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_DURATION_SOP

**Variable Label :** Time in minutes during which sample was centrifuged.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('CENTRI1_DURATION_SOP')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},t.map({'NA':null},t))
```



Time to first centrifugation or the sample is processed
---

**Variable Name  :** SAMPLE_CENTRI1_PRE_DELAY

**Variable Label :** Time between sample collection and first centrifugation. Calculated as the first centrifugation start time minus the sample collection time.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('CENTRI1_PRE_DELAY')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},t.map({'NA':null},t))
```



Temperature after first centrifugation (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_POST_TEMP

**Variable Label :** Temperature at which the primary source sample is temporary stored at post first centrifugation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('CENTRI1_POST_TEMP')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},t.map({'NA':null},t.value()))
```



Second centrifugation not applicable (SOP)
---

**Variable Name  :** SAMPLE_CENTRI2_NA

**Variable Label :** Documentation variable indicating if a second centrifugation was performed.  In the case of CPTP samples, second centrifugation did not occur.Information derived from SOPs.This variable is used to calculate SPREC code.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('CENTRI2_NA')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},t.map({'NA':null},t.value()))
```



Time from processing to short term storage
---

**Variable Name  :** SAMPLE_CENTR1_POST_DELAY

**Variable Label :** The time between the end of centrifugation and the time of short term storage. Calculated as sample short term storage time minus centrifugation end time.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('CENTR1_POST_DELAY')
$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':'-7',
  '7':'-7',
'6':'-7'},t.map({'NA':null},t.value()))
```



Sample process start date
---

**Variable Name  :** SAMPLE_PROCESS_DATE

**Variable Label :** Date the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SAMPLE_PROCESS_DATE')
t.map({'NA':null},t).date('yyyy-MM-dd')
```



Sample process start time
---

**Variable Name  :** SAMPLE_PROCESS_TIME

**Variable Label :** Time the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
}.call(null,$('SAMPLE_PROCESS_TIME'))

t
```



Sample process start date and time
---

**Variable Name  :** SAMPLE_PROCESS_DATETIME

**Variable Label :** Date and time the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_PROCESS_DATE'),$this('SAMPLE_PROCESS_TIME')]
.filter(function(x){return x.isNull().not().value()})

t.length == 2 ? t.reduce(function(x,y){return x.concat(' ',y)
                                               .datetime('yyyy-MM-dd HH:mm:ss')
                                               .format('yyyy-MM-dd HH:mm:ss')
                                               }) : null
```



Enrichment method (SOP)
---

**Variable Name  :** SAMPLE_ENRICHMENT_METHOD_SOP

**Variable Label :** Enrichment method used to obtain stored sample.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_ENRICHMENT_METHOD1')
//$this('SAMPLE_COLLECT_CONTAINER').map({
//2:1,
//3:1,
//1:1
//},6,t.map({
//'NA':6,
//'-7':'6'},t.value()))


```



Enrichment method
---

**Variable Name  :** SAMPLE_ENRICHMENT_METHOD

**Variable Label :** Enrichment method used to obtain stored sample.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_ENRICHMENT_METHOD2')
//$this('SAMPLE_COLLECT_CONTAINER').map({
//2:1,
//3:1,
//1:1
//},6,t.map({
//'NA':6,
//'-7':'6'},t.value()))
```



Impact on sample processing
---

**Variable Name  :** SAMPLE_PROCESS_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_PROCESS_FACTOR_YN')
```



Information critical to sample processing
---

**Variable Name  :** SAMPLE_PROCESS_FACTOR

**Variable Label :** Factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_PROCESS_FACTOR')
t.map({'NA':null},t)
```



Information critical to sample processing, other
---

**Variable Name  :** SAMPLE_PROCESS_NOTE

**Variable Label :** Factor that may have influenced sample processing, other.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_PROCESSING_NOTE')
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
  '7':'-7',
'6':'-7'},$('SAMPLE_LIPIDEMIA_PRESENT'))
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
  '7':'-7',
'6':'-7'},$('SAMPLE_HEMOLYSIS_PRESENT'))
```



Presence of hemoturia
---

**Variable Name  :** SAMPLE_HEMOTURIA_PRESENT

**Variable Label :** Hemoturia was observed at sample processing.It indicates whether blood was visually present in the urine sample.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_HEMOTURIA_PRESENT')

$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':t,
  '7':t,
'6':t},'-7')
```



Presence of turbidity
---

**Variable Name  :** SAMPLE_TURBIDITY_PRESENT

**Variable Label :** Turbidity was observed at sample processing.It indicates whether turbidity was observed within the urine sample.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_TURBIDITY_PRESENT')
var tb = t.map({'NA':null},t.value())

$this('SAMPLE_COLLECT_CONTAINER').map({
  '5':tb,
  '7':tb,
'6':tb},'-7')


```



Presence of other characteristic
---

**Variable Name  :** SAMPLE_OTHER_CHAR_PRESENT

**Variable Label :** Other characteristic was observed at sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
$('OTHER_CHARACTERISTIC_PRESENT')
```



Short Term (ST) storage time (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME_SOP

**Variable Label :** Time the sample is stored for short term.Information derived from SOPs.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_ST_STORAGE_TIME_SOP')
```



Short Term (ST) storage date
---

**Variable Name  :** SAMPLE_ST_STORAGE_DATE

**Variable Label :** Date the sample is stored for short term.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SAMPLE_ST_STORAGE_DATE')
t.map({'NA':null},t).date('yyyy-MM-dd')
```



Short Term (ST) storage time 
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME

**Variable Label :** Time the sample is stored for short term.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
}.call(null,$('SAMPLE_ST_STORAGE_TIME').trim())

t

```



Short Term (ST) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TEMP_SOP

**Variable Label :** Short term (ST) temperature at which the prepared or processed sample (e.g. serum) was stored temporarily prior to long-term storage.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_ST_STORAGE_TEMP_SOP')
t.map({'NA':null},t.value())
```



Second Short Term (ST) storage date
---

**Variable Name  :** SAMPLE_ST_STORAGE_DATE2

**Variable Label :** Date the sample is stored for second short term.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SAMPLE_ST_STORAGE_DATE2')
t.map({'NA':null},t.value())
```



Second Short Term (ST) storage time 
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME2

**Variable Label :** Time the sample is stored for second short term.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SAMPLE_ST_STORAGE_TIME2')
t.map({'NA':null},t.value())
```



Second Short Term (ST) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TEMP2

**Variable Label :** Second Short term (ST) temperature at which the prepared or processed sample (e.g. serum) was stored temporarily prior to long-term storage.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_ST_STORAGE_TEMP2')
t.map({'NA':null},t.value())
```



Sample shipping method (SOP)
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_METHOD

**Variable Label :** Method used to ship sample to the biobank (once the sample is processed). Information derived from SOPS.

**Value Type     :** integer

**JS script**

```{javascript}
$('SHIPMENT_TO_BIOBANK_METHOD')
```



Sample shipment date to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_DATE

**Variable Label :** Date the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_SHIPMENT_TO_BIOBANK_DATE').date('yyyy-MM-dd')
```



Sample shipment time to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_TIME

**Variable Label :** Time the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
}.call(null,$('SAMPLE_SHIPMENT_TO_BIOBANK_TIME').trim())

t
```



Sample shipment date and time to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_DATETIME

**Variable Label :** Date and time the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_SHIPT_BIOBANK_DATE'),$this('SAMPLE_SHIPT_BIOBANK_TIME')]
.filter(function(x){return x.isNull().not().value()})

t.length == 2 ? t.reduce(function(x,y){return x.concat(' ',y)
                                               .datetime('yyyy-MM-dd HH:mm:ss')
                                               .format('yyyy-MM-dd HH:mm:ss')
                                               }) : null
```



Sample arrival date at biobank storage facility
---

**Variable Name  :** SAMPLE_BIOBANK_ARRIVAL_DATE

**Variable Label :** Date the processed sample arrived at the biobank storage facility (e.g. Chicoutimi Biobank, Alberta biorepository).

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_BIOBANK_ARRIVAL_DATE').date('yyyy-MM-dd')
```



Sample arrival time at biobank storage facility
---

**Variable Name  :** SAMPLE_BIOBANK_ARRIVAL_TIME

**Variable Label :** Time the processed sample arrived at the biobank storage facility (e.g. Chicoutimi Biobank, Alberta biorepository).

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
}.call(null,$('SAMPLE_BIOBANK_ARRIVAL_TIME').trim())

t
```



Sample arrival date and time at biobank storage facility
---

**Variable Name  :** SAMPLE_BIOBANK_ARRIVAL_DATETIME

**Variable Label :** Date and time the processed sample arrived at the biobank storage facility (e.g. Chicoutimi Biobank, Alberta biorepository).

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_BIOBANK_ARRIVAL_DATE'),$this('SAMPLE_BIOBANK_ARRIVAL_TIME')]
.filter(function(x){return x.isNull().not().value()})

t.length == 2 ? t.reduce(function(x,y){return x.concat(' ',y)
                                               .datetime('yyyy-MM-dd HH:mm:ss')
                                               .format('yyyy-MM-dd HH:mm:ss')
                                               }) : null
```



Sample shipping duration to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_DURATION

**Variable Label :** The calculated time between sample arrival time at biobank storage facility minus sample shipment time.

**Value Type     :** text

**JS script**

```{javascript}
[$('SHIPMENT_TO_BIOBANK_DURATION')].map(function(x){
  return x.isNull().value() ? null : msToTime(x * 3600000)}).pop()

function msToTime(ms) {// convert millseconds duration into HH:mm:ss.SSSS format

    if(ms==null) { return ms}
    function isneg(x) {return x < 0? "-" : '' }
    ms_abs = Math.abs(ms)
    t = ms_abs/(1000*60*60)
    hrs =  Math.trunc(t)
    min =  Math.trunc((t-hrs)*60)
    sec =  Math.round((((t-hrs)*60)-min)*60)
    
    hrs = hrs < 10 ?  "0"+hrs: hrs;
    min = min < 10 ?  "0"+min : min;
    sec = sec < 10 ? "0" + sec : sec;
 
    k = isneg(ms) + hrs + ":" + min + ":" + sec
    return newValue(k,'text')
}
```



Sample transport to biobank storage facility temperature within range
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_TEMP

**Variable Label :** Indicator of whether or not temperature remained within required range during the transport from processing lab to biobank.The information can be issued from a data logger in the shipped box or from observation at sample arrival to the biobank (e.g. the sample is cold).

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SHIPMENT_TO_BIOBANK_TEMP_RANGE')
t.map({'NA':null},t.value())
```



Information critical about sample transport to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_FACTOR

**Variable Label :** Factor present that may have influenced the transport of sample from processing lab to biobank.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SHIPMENT_TO_BIOBANK_FACTOR')
t.map({'NA':null},t.value())
```



Long Term (LT) storage date
---

**Variable Name  :** SAMPLE_LT_STORAGE_DATE1

**Variable Label :** Date the sample is stored for long term.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_LT_STORAGE_DATE1').date('yyyy-MM-dd')
```



Long Term (LT) storage time
---

**Variable Name  :** SAMPLE_LT_STORAGE_TIME1

**Variable Label :** Time the sample is stored for long term.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
}.call(null,$('SAMPLE_LT_STORAGE_TIME1').trim())

t
```



Long Term (LT) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_LT_STORAGE_TEMP1

**Variable Label :** Long term (LT) temperature at which the prepared or processed sample (e.g. serum) was stored.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_LT_STORAGE_TEMP1')
t.map({'NA':null},t.value())
```



Sample initial freezing date
---

**Variable Name  :** SAMPLE_INI_FREEZE_DATE

**Variable Label :** Date on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_INI_FREEZE_DATE').date('yyyy-MM-dd')
```



Sample initial freezing time
---

**Variable Name  :** SAMPLE_INI_FREEZE_TIME

**Variable Label :** Time on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().value() ? null : 
  String(x).split(':',3).map(function(x){return x.length == 1 ? '0'+x : x}).join(':')
}.call(null,$('SAMPLE_INI_FREEZE_TIME').trim())

t
```



Sample initial freezing date and time
---

**Variable Name  :** SAMPLE_INI_FREEZE_DATETIME

**Variable Label :** Date and time on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_INI_FREEZE_DATE'),$this('SAMPLE_INI_FREEZE_TIME')]
.filter(function(x){return x.isNull().not().value()})

t.length == 2 ? t.reduce(function(x,y){return x.concat(' ',y)
                                               .datetime('yyyy-MM-dd HH:mm:ss')
                                               .format('yyyy-MM-dd HH:mm:ss')
                                               }) : null
```



Time to initial freezing
---

**Variable Name  :** SAMPLE_COLLECT_INI_FREEZE_TIME

**Variable Label :** Time between time of source sample collection (e.g. whole blood) and time the sample (e.g. serum) is first frozen for long term storage.Calculated as date and time the sample is first frozen minus date and time of sample collection.

**Value Type     :** text

**JS script**

```{javascript}
[$('COLLECT_TO_INI_FREEZE_TIME')]
.map(function(x){
  return x.isNull().value() ? null : msToTime(x * 3600000)}).pop()

function msToTime(ms) {// convert millseconds duration into HH:mm:ss.SSSS format

    if(ms==null) { return ms}
    function isneg(x) {return x < 0? "-" : '' }
    ms_abs = Math.abs(ms)
    t = ms_abs/(1000*60*60)
    hrs =  Math.trunc(t)
    min =  Math.trunc((t-hrs)*60)
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
var t = $('SAMPLE_LT_STORAGE_AGENT')
t.map({'NA':null},t.value())
```



Container Type
---

**Variable Name  :** SAMPLE_LT_STORAGE_CONTAINER

**Variable Label :** Tube in which the sample type is stored in for long term storage.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_LT_STORAGE_CONTAINER')
t.map({'NA':null},t.value())
```



Second long term (LT) storage date
---

**Variable Name  :** SAMPLE_LT_STORAGE_DATE2

**Variable Label :** Date the sample is moved to a different long term storage conditions after being removed from its first long term storage conditions.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SAMPLE_LT_STORAGE_DATE2')
t.map({'NA':'-7'},t)
```



Second long term (LT) storage time
---

**Variable Name  :** SAMPLE_LT_STORAGE_TIME2

**Variable Label :** Time the sample is moved to a different long term storage conditions after being removed from its first long term storage conditions.

**Value Type     :** text

**JS script**

```{javascript}
var t = $('SAMPLE_LT_STORAGE_TIME2')
t.map({'NA':'-7'},t.value())
```



Second long term (LT) storage temperature 
---

**Variable Name  :** SAMPLE_LT_STORAGE_TEMP2

**Variable Label :** A different long term (LT) storage temperature the sample type is stored after being removed from its first long term storage conditions.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('SAMPLE_LT_STORAGE_TEMP2')
t.map({'NA':'-7'},t.value())
```



Number of freeze-thaw cycles
---

**Variable Name  :** SAMPLE_FREEZE_THAW

**Variable Label :** Number of times a frozen sample has been thawed and then refrozen.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_FREEZE_THAW_CYCLES')
```



Number of freeze-thaw cycles - theoritical
---

**Variable Name  :** SAMPLE_FREEZE_THAW_THEORITICAL

**Variable Label :** Theoritical number of times a frozen sample has been thawed and then refrozen. The information is not captured in the LIMS database but derived from another source of information.

**Value Type     :** integer

**JS script**

```{javascript}
$('FREEZE_THAW_CYCLES_THEORITICAL')
```



Sample thaw method (SOP)
---

**Variable Name  :** SAMPLE_THAW_TEMP

**Variable Label :** The method used, whether intentional or not, that resulted in the sample thawing as per an SOP.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_THAW_TEMP')
```



Impact on sample storage
---

**Variable Name  :** SAMPLE_STORAGE_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample storage.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_STORAGE_FACTOR_YN')
```



Information critical about sample storage
---

**Variable Name  :** SAMPLE_STORAGE_FACTOR

**Variable Label :** Factor that may have influenced sample storage.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_STORAGE_FACTOR')
```



Sample volume measured
---

**Variable Name  :** SAMPLE_VOLUME_MEASURED

**Variable Label :** Sample volume recorded.

**Value Type     :** decimal

**JS script**

```{javascript}
var t = $('SAMPLE_VOLUME_MEASURED')
t.map({'NA':null},t)
```



Sample volume theoritical
---

**Variable Name  :** SAMPLE_VOLUME_THEORITICAL

**Variable Label :** Sample volume theoritical (i.e. volume range).

**Value Type     :** decimal

**JS script**

```{javascript}
var t = $('SAMPLE_VOLUME_THEORITICAL')
t.map({
  '768 wells' : null, //recoded '768 wells' to null
  '5 ml' : 5,
  '4 ml' : 4,
  '3 ml' : 3,
  '2.5 ml' : 2.5,
  '2.0 ml' : 2,
  '2 ml tubes': 2,
  '2 ml cryovial' : 2,
  '1 tube (volume variable)' : null
},null)*1000
```



Sample volume that has been taken off
---

**Variable Name  :** SAMPLE_VOLUME_REMOVED

**Variable Label :** The volume taken from the initial sample volume.Calculated as  the sum of the different volumes that has been taken of the initial sample volume.

**Value Type     :** decimal

**JS script**

```{javascript}
var t = $('SAMPLE_VOLUME_REMOVED')
t.map({'NA':null},t)
```



Sample volume remaining
---

**Variable Name  :** SAMPLE_VOLUME_REMAINING

**Variable Label :** Sample volume remaining after each use.Calculated as the sample volume (either measured or theoritical) minus the sample volume that has been taken off.

**Value Type     :** decimal

**JS script**

```{javascript}
var t = $('SAMPLE_VOLUME_REMAINING')
t.map({'NA':null},t)
```



SPREC code
---

**Variable Name  :** SAMPLE_SPREC_CODE

**Variable Label :** The Sample PREanalytical code (SPREC) is a standard code used to describe the biosample preanalytical procedures.For the fluid biosample, the following information are described:-Type of sample (char 1-3)-Type of primary container (char 5-7)-Pre-centrifugation (char 9)-Centrifigation (char 11)-Second centrifugation (char 13)-Post centrifugation (char 15)-Storage condition  (char 17)Information is calculated using the associated variables.Reference: Betsou et al. 2010

**Value Type     :** text

**JS script**

```{javascript}
var t0 = $('SAMPLE_SPREC_CODE')

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

t0.whenNull(t.reduce(function(x,y){return x+'-'+y}))

    
    


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




