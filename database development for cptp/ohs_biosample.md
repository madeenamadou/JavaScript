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
$('Source_Ref:PRIMARY_TUBE_POOLED').map({
  'Primary source tubes were not pooled':0
  },0)//No sample were pooled.
```



Sample collection container
---

**Variable Name  :** SAMPLE_COLLECT_CONTAINER

**Variable Label :** Container in which the sample was collected in.

**Value Type     :** integer

**JS script**

```{javascript}
$('OHS_R2_SAMPLE.Source_Ref:Type').trim().map({
  '10mL lavender top EDTA tube':3,
  '8.5mL mottled top SST tube':2,
  'Saliva OG-250':5,
  "Saliva OG-500":6,
  'Urine':7
},
//there were 35 NAs due to these tubes missing in source dataset
$('CPAC_Source_ID').map({
'OHSLL167237EDT1':3,
'OHSLL264793EDT1':3,
'OHSLL344545EDT1':3,
'OHSLL38061EDT1':3,
'OHSLL39777EDT1':3,
'OHSLL542059EDT2':3,
'OHSLL542059EDT1':3}))
```



Sample type
---

**Variable Name  :** SAMPLE_TYPE

**Variable Label :** Indicator of the type of sample being referenced.

**Value Type     :** integer

**JS script**

```{javascript}
$('Sample_TYPE').trim().map({
  'Plasma, single spun':5,
  'Red blood cells':4,
  'Serum':3,
  'Unficolled buffy coat, nonviable':7,
  'Urine':10
  })

//Saliva missing from primary dataset
//95 tubes from source are missing in primary
//                          Type   
// 10mL lavender top EDTA tube: 9  
// 8.5mL mottled top SST tube :46  
// Saliva OG-250              : 1  
// Saliva OG-500              :24  
// Urine                      :15
```



Date of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_DATE

**Variable Label :** Date on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().map({
    false : String(x).split(' ',2)[0].split('-',3).
    map(function(x){return  x.length ==  1 ? '0'+ x : x}).join('-')
  },null).date('yyyy-MM-dd')
  }.call(null,$('OHS_R2_SAMPLE.Source_Ref:CreateTime'))

t
```



Time of the sample collection
---

**Variable Name  :** SAMPLE_COLLECT_TIME

**Variable Label :** Time on which the biological sample is collected.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
 return x.isNull().map({
 false : String(x).slice(0, -2).split(' ',2)[1]
 },null).value()
}.call(null,$('OHS_R2_SAMPLE.Source_Ref:CreateTime'))

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
$('Source_Ref:CreateCentre')
```



Participant vital state (SOP)
---

**Variable Name  :** SAMPLE_VITAL_STATE

**Variable Label :** Documentation variable indicating the state of the participant at time of sample collection. In the CPTP context, all participants were alive at time of sample donation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('Source_Ref:VITAL_STATE').map({
  'Alive':1
  },1)
```



Sample collection body site (SOP)
---

**Variable Name  :** SAMPLE_COLLECT_BODY_SITE

**Variable Label :** Body site of sample collection.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  5 : 2,
  6 : 2,
  7 : 3
},1,$('Source_Ref:SAMPLE_COLLECT_BODY_SITE').map({
  'Arm venipuncture':1,
  'Urethra':3,
  'Mouth':2
  },null))
```



Sample collection method (SOP)
---

**Variable Name  :** SAMPLE_COLLECT_METHOD

**Variable Label :** Collection device or parameters used during sample collection.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_TYPE').map({
  '9':3,
  '10':2
},1,$('Source_Ref:SAMPLE_COLLECT_METHOD').map({
  '21G - 23G Needle':1,
  'Mid-stream random sample':2,
  '30 minutes post eat/drink':3
  },null))


```



Volume collected sufficient for biological sample extraction
---

**Variable Name  :** SAMPLE_VOL_SUF_EXTRACT

**Variable Label :** Sufficient volume collected per primary tube from which the biological sample will be extracted, according to manufacturer's instructions.

**Value Type     :** integer

**JS script**

```{javascript}
$('OHS_R2_SAMPLE.Source_Ref:Comments').map({
  "less volume":0,
  "Less volume":0,
  "Low EDTA tube volume":0,
  "SST 1 has less serum volume. Only filled 4 matrix tubes":0,
  "Less than 7 ml":0,
  "Less than 5 ml in EDTA" :0,
  "< 6 ml\nLess than 10ml sample received at PHO on 2012.08.11":0,
  "SST 1 has less serum volume. Only filled 4 matrix tubes":0,
  "Low EDTA tube volume":0,
  "insufficient":0,
  "Blood Sample was collected insufficiently due to collapsed vein, participant doesn't want to be poke again.":0,
  "insufficient sample due to collapsed vein":0,
  "insufficient sample due to collapsed vein.":0
  },null,1)
```



Saliva collection device model
---

**Variable Name  :** SAMPLE_SALIVA_COLLECT_MODEL

**Variable Label :** The model number of the saliva sample collection device.

**Value Type     :** text

**JS script**

```{javascript}
//inconsistent results with sample type
$this('SAMPLE_TYPE').map({
  '9':'OG-250',
},$('Source_Ref:SALIVA_COLLECT_DEVICE_MODEL').map({
  'Oragene Kit':'OG-250'},null))
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
$('Source_Ref:SAMPLE_STAB_TIME').map({
  '30-60 minutes':3,
  '<10 minutes':1,
  'Unknown':9
  },$('CPAC_Source_ID').map({
'OHSLL167237EDT1':1,
'OHSLL264793EDT1':1,
'OHSLL344545EDT1':1,
'OHSLL38061EDT1':1,
'OHSLL39777EDT1':1,
'OHSLL542059EDT2':1,
'OHSLL542059EDT1':1}))
```



Temperature between biospecimen acquisition and initial stabilization (SOP)
---

**Variable Name  :** SAMPLE_PRE_STAB_TEMP

**Variable Label :** Temperature at which the sample was stored at prior to stabilization as noted in the SOP.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
1//Should be RT for all samples
```



Mechanism of sample stabilization (SOP)
---

**Variable Name  :** SAMPLE_STAB_METHOD

**Variable Label :** Method used to stabilize the sample.Information derived from SOPs. 

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  '3':1,
  '1':1,
  '2':2
},$('Source_Ref:SAMPLE_STAB_METHOD').map({
  'Additive':1,
  '4 degrees Celsius & Additive(EDTA)':1,
  '4 degrees Celsius':3,
  'Additive (Saliva)':1,
  'Unknown':'9'}))
```



Temperature post stabilization or temporary storage temperature (SOP)
---

**Variable Name  :** SAMPLE_POST_STAB_TEMP

**Variable Label :** Temperature at which the sample was temporarily stored post stabilization.When the sample is centrifuged, it refers to the temperature at which the primary source sample is temporary stored prior first centrifugation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('Source_Ref:SAMPLE_POST_STAB_TEMP').map({
  'Ambient Temperature':1,
  '4 degrees Celsius':2
  },$('CPAC_Source_ID').map({
'OHSLL167237EDT1':1,
'OHSLL264793EDT1':1,
'OHSLL344545EDT1':1,
'OHSLL38061EDT1':1,
'OHSLL39777EDT1':1,
'OHSLL542059EDT2':1,
'OHSLL542059EDT1':1}))
```



Tracking number of the shipment to off-site processing lab
---

**Variable Name  :** SAMPLE_SHIPT_TRACK_NO

**Variable Label :** Tracking number of the shipment to off-site processing lab. 

**Value Type     :** text

**JS script**

```{javascript}
$('Source_Ref:ShipID')
```



Sample shipping method to off-site processing lab (SOP)
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_METHOD

**Variable Label :** Method used to ship source samples to the processing lab.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('Source_Ref:SAMPLE_SHIPMENT_TO_PROCESS_LAB_METHOD').map({
  'Ice pack or Cold/Gel pack' : 5
  },$('CPAC_Source_ID').map({
'OHSLL167237EDT1':5,
'OHSLL264793EDT1':5,
'OHSLL344545EDT1':5,
'OHSLL38061EDT1':5,
'OHSLL39777EDT1':5,
'OHSLL542059EDT2':5,
'OHSLL542059EDT1':5}))
```



Sample transport temperature within range
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_TEMP

**Variable Label :** Indicator of whether or not temperature remained within required range during the transport from collection site to proccesing lab.The information can be issued from a data logger in the shipped box or from observation at sample arrival to processing lab (e.g. the sample is not cold).

**Value Type     :** integer

**JS script**

```{javascript}
//The information is missing for baseline participants
```



Information critical about sample transport
---

**Variable Name  :** SAMPLE_SHIPT_PROCESS_LAB_FACTOR

**Variable Label :** Factor present that may have influenced the transport of sample from collection site to processing lab.

**Value Type     :** text

**JS script**

```{javascript}
//Missing
```



Sample arrival date at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_DATE

**Variable Label :** Date the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().map({
    false : String(x).split(' ',2)[0].split('-',3).
    map(function(x){return  x.length ==  1 ? '0'+ x : x}).join('-')
  },null).date('yyyy-MM-dd')
  }.call(null,$('OHS_R2_SAMPLE.Source_Ref:ShipRcvTime'))

t
```



Sample arrival time at processing lab
---

**Variable Name  :** SAMPLE_ARRIVAL_TIME

**Variable Label :** Time the sample source tube arrived at the processing lab.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
 return x.isNull().map({
 false : String(x).slice(0, -2).split(' ',2)[1]
 },null)
}.call(null,$('OHS_R2_SAMPLE.Source_Ref:ShipRcvTime'))

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
var t = [$this('SAMPLE_ARRIVAL_DATETIME'),$this('SAMPLE_COLLECT_DATETIME')]
.filter(function(x){return x.isNull().not().value()})
.map(function(x){return x.datetime('yyyy-MM-dd HH:mm:ss').value()})

t.length == 2 ? t.reduce(function(x,y){return x-y<0 ? null : msToTime(x-y)}) : null

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



First centrifugation start time (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_START_TIME_SOP

**Variable Label :** Time between sample collection and sample first centrifugation.Information derived from SOPS.

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : '30 min after collection (<2h)',
  3 : '<24 hours after collection',
},'-7',$('OHS_R2_SAMPLE.Source_Ref:CENTRI1_START_TIME_SOP').whenNull('99'))
```



First centrifugation start date
---

**Variable Name  :** SAMPLE_CENTRI1_START_DATE

**Variable Label :** Date when first centrifugation was performed.Calculated from end date and time minus spin delay. 

**Value Type     :** text

**JS script**

```{javascript}
var d = $this('SAMPLE_PROCESS_DATE')
$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : d,
  3 : d
  },'-7')

```



First centrifugation start time
---

**Variable Name  :** SAMPLE_CENTRI1_START_TIME

**Variable Label :** Time when first centrifugation was performed.Calculated from end date and time minus spin delay. 

**Value Type     :** text

**JS script**

```{javascript}
var t = $this('SAMPLE_PROCESS_TIME')
$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : t,
  3 : t
  },'-7')
```



First centrifugation speed (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_GFORCE

**Variable Label :** G-force speed at which first centrifugation was performed.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
// unkown for OHSLL167237EDT1 OHSLL264793EDT1 OHSLL344545EDT1 
// OHSLL38061EDT1  OHSLL39777EDT1  OHSLL542059EDT1 OHSLL542059EDT2
var t = $('OHS_R2_SAMPLE.Source_Ref:CENTRI1_GFORCE').map({
  '1300g':1300,
  '2000g':2000,
  '3000g':3000},'99999')

$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : t,
  3 : t
  },'-7')
```



First centrifugation brake (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_BRAKE

**Variable Label :** Whether or not the centrifugation break was used.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : '9',
  3 : '9'
  },'-7')
```



First centrifugation temperature (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_TEMP

**Variable Label :** Temperature at which the first centrifugation was performed.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
//35 unkown
//Not all LSC samples were centrifuged at 4 degrees. 
//We cannot say definitively which were and which were not. 
//We can say that the LSC's were either centrifuged at 4’ or RT. 
//For this variable it’s best to record as Unknown and provide further 
//explanation to the requesting researcher.
var t = $('OHS_R2_SAMPLE.Source_Ref:CENTRI1_TEMP').map({
  'Room temperature (RT)' : 1,
  '4 degrees Celsius' : 2},'9')

$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : $('Source_Ref:Workflow').map({'LSC':1,'BC':1,'AC':2},t),
  3 : $('Source_Ref:Workflow').map({'LSC':'9','BC':1,'AC':2},t)
  },'-7')
```



First centrifugation end date
---

**Variable Name  :** SAMPLE_CENTRI1_END_DATE

**Variable Label :** Date when first centrifugation ended.

**Value Type     :** text

**JS script**

```{javascript}
var t = $this('SAMPLE_PROCESS_DATE')
$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : t,
  3 : t
  },'-7')
```



First centrifugation end time
---

**Variable Name  :** SAMPLE_CENTRI1_END_TIME

**Variable Label :** Time when first centrifugation ended.

**Value Type     :** text

**JS script**

```{javascript}
//No time available
$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : null,
  3 : null
  },'-7')

```



First centrifugation duration (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_DURATION_SOP

**Variable Label :** Time in minutes during which sample was centrifuged.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('OHS_R2_SAMPLE.Source_Ref:CENTRI1_DURATION_SOP').trim().map({
  '10min':10,
  '15min':15
  },$('CPAC_Source_ID').map({
'OHSLL167237EDT1':15,
'OHSLL264793EDT1':15,
'OHSLL344545EDT1':15,
'OHSLL38061EDT1':15,
'OHSLL39777EDT1':15,
'OHSLL542059EDT2':15,
'OHSLL542059EDT1':15}),'99')

$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : t,
  3 : t
  },'-7')

```



Time to first centrifugation or the sample is processed
---

**Variable Name  :** SAMPLE_CENTRI1_PRE_DELAY

**Variable Label :** Time between sample collection and first centrifugation. Calculated as the first centrifugation start time minus the sample collection time.

**Value Type     :** text

**JS script**

```{javascript}
//35 unkown
var t = $('OHS_R2_SAMPLE.Source_Ref:CENTRI1_PRE_DELAY')

$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : t,
  3 : t
  },'-7')
```



Temperature after first centrifugation (SOP)
---

**Variable Name  :** SAMPLE_CENTRI1_POST_TEMP

**Variable Label :** Temperature at which the primary source sample is temporary stored at post first centrifugation.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
var t = $('OHS_R2_SAMPLE.Source_Ref:CENTRI1_POST_TEMP').map({
  '4 degrees Celsius':2},1,$('CPAC_Source_ID').map({
'OHSLL167237EDT1':2,
'OHSLL264793EDT1':2,
'OHSLL344545EDT1':2,
'OHSLL38061EDT1':2,
'OHSLL39777EDT1':2,
'OHSLL542059EDT2':2,
'OHSLL542059EDT1':2}),'9')

$this('SAMPLE_COLLECT_CONTAINER').map({
  2 : t,
  3 : t
  },'-7')
```



Second centrifugation not applicable (SOP)
---

**Variable Name  :** SAMPLE_CENTRI2_NA

**Variable Label :** Documentation variable indicating if a second centrifugation was performed.  In the case of CPTP samples, second centrifugation did not occur.Information derived from SOPs.This variable is used to calculate SPREC code.

**Value Type     :** integer

**JS script**

```{javascript}
$('OHS_R2_SAMPLE.Source_Ref:CENTRI2_NA').map({
  'No second centrifugation':-7},'-7','-7')
```



Time from processing to short term storage
---

**Variable Name  :** SAMPLE_CENTR1_POST_DELAY

**Variable Label :** The time between the end of centrifugation and the time of short term storage. Calculated as sample short term storage time minus centrifugation end time.

**Value Type     :** text

**JS script**

```{javascript}
//Impossible
```



Sample process start date
---

**Variable Name  :** SAMPLE_PROCESS_DATE

**Variable Label :** Date the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().map({
    false : String(x).split(' ',2)[0].split('-',3).
    map(function(x){return  x.length ==  1 ? '0'+ x : x}).join('-')
  },null).date('yyyy-MM-dd')
  }.call(null,$('CreateTime'))

t

```



Sample process start time
---

**Variable Name  :** SAMPLE_PROCESS_TIME

**Variable Label :** Time the sample is processed. The processing lab may be the same as collection site or different.  For the samples that were centrifuged (serum and EDTA vacutainer tubes), the processing time is the same as ''CENTRI1_START_TIME''.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
 return x.isNull().map({
 false : String(x).split(' ',2)[1].slice(0, -2)
 },null)
}.call(null,$('CreateTime'))

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
$this('SAMPLE_TYPE').map({
  1 : 6,
  3 : 1,
  4 : 1,
  5 : 1,
  7 : 1,
  8 : 4,
  9 : 6,
  10 : 6
})
```



Enrichment method
---

**Variable Name  :** SAMPLE_ENRICHMENT_METHOD

**Variable Label :** Enrichment method used to obtain stored sample.

**Value Type     :** integer

**JS script**

```{javascript}
$('OHS_R2_SAMPLE.Source_Ref:SAMPLE_ENRICHMENT_METHOD').map({
  'Centrifugation':1,
  'None':6
  },$this('SAMPLE_TYPE').map({
  1 : 6,
  3 : 1,
  4 : 1,
  5 : 1,
  7 : 1,
  8 : 4,
  9 : 6,
  10 : 6
}))
```



Impact on sample processing
---

**Variable Name  :** SAMPLE_PROCESS_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//Not provided
```



Information critical to sample processing
---

**Variable Name  :** SAMPLE_PROCESS_FACTOR

**Variable Label :** Factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//Not provided
```



Information critical to sample processing, other
---

**Variable Name  :** SAMPLE_PROCESS_NOTE

**Variable Label :** Factor that may have influenced sample processing, other.

**Value Type     :** text

**JS script**

```{javascript}
//Not provided
```



Presence of lipidemia
---

**Variable Name  :** SAMPLE_LIPIDEMIA_PRESENT

**Variable Label :** Lipidemia was observed at sample processing.It indicates whether lipids were visually present in the plasma or serum portion of the source sample.

**Value Type     :** integer

**JS script**

```{javascript}
//Not collected
$this('SAMPLE_COLLECT_CONTAINER').map({
  '2':null,
  '3':null
  },'-7')
```



Presence of hemolysis
---

**Variable Name  :** SAMPLE_HEMOLYSIS_PRESENT

**Variable Label :** Hemolysis was observed at sample processing.It indicates whether blood was visually present in the plasma or serum portion of the source sample.

**Value Type     :** integer

**JS script**

```{javascript}
//Not collected
$this('SAMPLE_COLLECT_CONTAINER').map({
  '2':null,
  '3':null
  },'-7')
```



Presence of hemoturia
---

**Variable Name  :** SAMPLE_HEMOTURIA_PRESENT

**Variable Label :** Hemoturia was observed at sample processing.It indicates whether blood was visually present in the urine sample.

**Value Type     :** integer

**JS script**

```{javascript}
//Not collected
$this('SAMPLE_COLLECT_CONTAINER').map({
  '7':null,
  '5':null,
  '6':null
  },'-7')
```



Presence of turbidity
---

**Variable Name  :** SAMPLE_TURBIDITY_PRESENT

**Variable Label :** Turbidity was observed at sample processing.It indicates whether turbidity was observed within the urine sample.

**Value Type     :** integer

**JS script**

```{javascript}
//Not collected
$this('SAMPLE_COLLECT_CONTAINER').map({
  '7':null,
  '5':null,
  '6':null
  },'-7')
```



Presence of other characteristic
---

**Variable Name  :** SAMPLE_OTHER_CHAR_PRESENT

**Variable Label :** Other characteristic was observed at sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//Not collected
```



Short Term (ST) storage time (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME_SOP

**Variable Label :** Time the sample is stored for short term.Information derived from SOPs.

**Value Type     :** text

**JS script**

```{javascript}
$('SAMPLE_ST_STORAGE_TIME_SOP').map({
  'Depends on batch volume' : '99:99'},'99:99')
```



Short Term (ST) storage date
---

**Variable Name  :** SAMPLE_ST_STORAGE_DATE

**Variable Label :** Date the sample is stored for short term.

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_PROCESS_DATE')
```



Short Term (ST) storage time 
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME

**Variable Label :** Time the sample is stored for short term.

**Value Type     :** text

**JS script**

```{javascript}
//Not collected
```



Short Term (ST) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TEMP_SOP

**Variable Label :** Short term (ST) temperature at which the prepared or processed sample (e.g. serum) was stored temporarily prior to long-term storage.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_ST_STORAGE_TEMP_SOP').map({
  '-80 degrees Celsius' : 1
  },1)
```



Second Short Term (ST) storage date
---

**Variable Name  :** SAMPLE_ST_STORAGE_DATE2

**Variable Label :** Date the sample is stored for second short term.

**Value Type     :** text

**JS script**

```{javascript}
//Not applicable
'-7'
```



Second Short Term (ST) storage time 
---

**Variable Name  :** SAMPLE_ST_STORAGE_TIME2

**Variable Label :** Time the sample is stored for second short term.

**Value Type     :** text

**JS script**

```{javascript}
//Not applicable
'-7'
```



Second Short Term (ST) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_ST_STORAGE_TEMP2

**Variable Label :** Second Short term (ST) temperature at which the prepared or processed sample (e.g. serum) was stored temporarily prior to long-term storage.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
//Not applicable
'-7'
```



Sample shipping method (SOP)
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_METHOD

**Variable Label :** Method used to ship sample to the biobank (once the sample is processed). Information derived from SOPS.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_SHIPMENT_TO_BIOBANK_METHOD').map({
  'Vapor phase LN2' : 4
  })
```



Sample shipment date to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_DATE

**Variable Label :** Date the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
  return x.isNull().map({
    false : String(x).split(' ',2)[0].split('-',3).
    map(function(x){return  x.length ==  1 ? '0'+ x : x}).join('-')
  },null).date('yyyy-MM-dd')
  }.call(null,$('Disp_1_SndTime_calc'))

t
```



Sample shipment time to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_TIME

**Variable Label :** Time the processed sample is shipped to biobank.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
 return x.isNull().map({
 false : String(x).slice(0, -2).split(' ',2)[1]
 },null).value()
}.call(null,$('Disp_1_SndTime_calc'))

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
var t = function(x){
  return x.isNull().map({
    false : String(x).split(' ',2)[0].split('-',3).
    map(function(x){return  x.length ==  1 ? '0'+ x : x}).join('-')
  },null).date('yyyy-MM-dd')
  }.call(null,$('Disp_1_RcvTime'))

t
```



Sample arrival time at biobank storage facility
---

**Variable Name  :** SAMPLE_BIOBANK_ARRIVAL_TIME

**Variable Label :** Time the processed sample arrived at the biobank storage facility (e.g. Chicoutimi Biobank, Alberta biorepository).

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
 return x.isNull().map({
 false : String(x).slice(0, -2).split(' ',2)[1]
 },null)
}.call(null,$('Disp_1_RcvTime'))

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
var t = [$this('SAMPLE_BIOBANK_ARRIVAL_DATETIME'),$this('SAMPLE_SHIPT_BIOBANK_DATETIME')]
.filter(function(x){return x.isNull().not().value() && x.eq('-7').not().value()})

t.length == 2 ? t.map(function(x){return x.datetime('yyyy-MM-dd HH:mm:ss').value()})
                          .reduce(function(x,y){return x-y<0 ? null : msToTime(x-y)}) : null

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



Sample transport to biobank storage facility temperature within range
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_TEMP

**Variable Label :** Indicator of whether or not temperature remained within required range during the transport from processing lab to biobank.The information can be issued from a data logger in the shipped box or from observation at sample arrival to the biobank (e.g. the sample is cold).

**Value Type     :** integer

**JS script**

```{javascript}
//!!??
```



Information critical about sample transport to biobank storage facility
---

**Variable Name  :** SAMPLE_SHIPT_BIOBANK_FACTOR

**Variable Label :** Factor present that may have influenced the transport of sample from processing lab to biobank.

**Value Type     :** text

**JS script**

```{javascript}
//!!??
```



Long Term (LT) storage date
---

**Variable Name  :** SAMPLE_LT_STORAGE_DATE1

**Variable Label :** Date the sample is stored for long term.

**Value Type     :** text

**JS script**

```{javascript}
//still lots of NAs
var t = function(x){
  return x.isNull().map({
    false : String(x).split(' ',2)[0].split('-',3).
    map(function(x){return  x.length ==  1 ? '0'+ x : x}).join('-')
  },null).date('yyyy-MM-dd')
  }.call(null,$('Disp_1_SndTime_calc')) //variable generated based on the original, to reduce the NAs
t
```



Long Term (LT) storage time
---

**Variable Name  :** SAMPLE_LT_STORAGE_TIME1

**Variable Label :** Time the sample is stored for long term.

**Value Type     :** text

**JS script**

```{javascript}
var t = function(x){
 return x.isNull().map({
 false : String(x).slice(0, -2).split(' ',2)[1]
 },null).value()
}.call(null,$('Disp_1_SndTime_calc'))

t
```



Long Term (LT) storage temperature (SOP)
---

**Variable Name  :** SAMPLE_LT_STORAGE_TEMP1

**Variable Label :** Long term (LT) temperature at which the prepared or processed sample (e.g. serum) was stored.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$this('SAMPLE_TYPE').map({
  '9':7
  },4,$('SAMPLE_LT_STORAGE_TEMP1').map({'Liquid Nitrogen (LN)' : 4}))
```



Sample initial freezing date
---

**Variable Name  :** SAMPLE_INI_FREEZE_DATE

**Variable Label :** Date on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
$this('SAMPLE_TYPE').map({
  '9':null
  },$this('SAMPLE_ST_STORAGE_DATE'))
```



Sample initial freezing time
---

**Variable Name  :** SAMPLE_INI_FREEZE_TIME

**Variable Label :** Time on which processed sample is first frozen for long-term storage. When the sample was first frozen on a short term storage step, the short term storage time is taken.

**Value Type     :** text

**JS script**

```{javascript}
//Not collected
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

var dt = t.length == 2 ? t.reduce(function(x,y){return x.concat(' ',y)
                                               .datetime('yyyy-MM-dd HH:mm:ss')
                                               .format('yyyy-MM-dd HH:mm:ss')
                                               }) : null
$this('SAMPLE_TYPE').map({
  '9':null
  },dt)
```



Time to initial freezing
---

**Variable Name  :** SAMPLE_COLLECT_INI_FREEZE_TIME

**Variable Label :** Time between time of source sample collection (e.g. whole blood) and time the sample (e.g. serum) is first frozen for long term storage.Calculated as date and time the sample is first frozen minus date and time of sample collection.

**Value Type     :** text

**JS script**

```{javascript}
var t = [$this('SAMPLE_INI_FREEZE_DATETIME'),$this('SAMPLE_COLLECT_DATETIME')]
.filter(function(x){return x.isNull().not().value() && x.eq('-7').not().value()})

t.length == 2 ? t.map(function(x){return x.datetime('yyyy-MM-dd HH:mm:ss').value()})
                          .reduce(function(x,y){return x-y<0 ? null : msToTime(x-y)}) : null

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
$this('SAMPLE_TYPE').map({
  '9':2
  },'-7',$('SAMPLE_LT_STORAGE_AGENT').map({'Not applicable' : '-7'}))
```



Container Type
---

**Variable Name  :** SAMPLE_LT_STORAGE_CONTAINER

**Variable Label :** Tube in which the sample type is stored in for long term storage.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_LT_STORAGE_CONTAINER').map({
  '2.0ml Greiner Cryovials' : 2,
  'Combination of the 2.0ml Greiner Cryovial and the 0.5mL Matrix Tube' : 2
  })
```



Second long term (LT) storage date
---

**Variable Name  :** SAMPLE_LT_STORAGE_DATE2

**Variable Label :** Date the sample is moved to a different long term storage conditions after being removed from its first long term storage conditions.

**Value Type     :** text

**JS script**

```{javascript}
//Not applicable
```



Second long term (LT) storage time
---

**Variable Name  :** SAMPLE_LT_STORAGE_TIME2

**Variable Label :** Time the sample is moved to a different long term storage conditions after being removed from its first long term storage conditions.

**Value Type     :** text

**JS script**

```{javascript}
//Not applicable
```



Second long term (LT) storage temperature 
---

**Variable Name  :** SAMPLE_LT_STORAGE_TEMP2

**Variable Label :** A different long term (LT) storage temperature the sample type is stored after being removed from its first long term storage conditions.

**Value Type     :** integer

**JS script**

```{javascript}
//Not applicable
```



Number of freeze-thaw cycles
---

**Variable Name  :** SAMPLE_FREEZE_THAW

**Variable Label :** Number of times a frozen sample has been thawed and then refrozen.

**Value Type     :** integer

**JS script**

```{javascript}
0//Sample has not been thawed
```



Number of freeze-thaw cycles - theoritical
---

**Variable Name  :** SAMPLE_FREEZE_THAW_THEORITICAL

**Variable Label :** Theoritical number of times a frozen sample has been thawed and then refrozen. The information is not captured in the LIMS database but derived from another source of information.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_FREEZE_THAW_CYCLES_THEORITICAL').whenNull(0)
```



Sample thaw method (SOP)
---

**Variable Name  :** SAMPLE_THAW_TEMP

**Variable Label :** The method used, whether intentional or not, that resulted in the sample thawing as per an SOP.Information derived from SOPs.

**Value Type     :** integer

**JS script**

```{javascript}
$('SAMPLE_THAW_TEMP').map({
  'Not applicable (sample has not been thawed)':'-7'
  },null)
```



Impact on sample storage
---

**Variable Name  :** SAMPLE_STORAGE_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample storage.

**Value Type     :** integer

**JS script**

```{javascript}
//Not available
```



Information critical about sample storage
---

**Variable Name  :** SAMPLE_STORAGE_FACTOR

**Variable Label :** Factor that may have influenced sample storage.

**Value Type     :** text

**JS script**

```{javascript}
//Not available
```



Sample volume measured
---

**Variable Name  :** SAMPLE_VOLUME_MEASURED

**Variable Label :** Sample volume recorded.

**Value Type     :** decimal

**JS script**

```{javascript}
var t = parseFloat($('Quantity'))
isNaN(t) ? null : t*1000
```



Sample volume theoritical
---

**Variable Name  :** SAMPLE_VOLUME_THEORITICAL

**Variable Label :** Sample volume theoritical (i.e. volume range).

**Value Type     :** decimal

**JS script**

```{javascript}
$('SAMPLE_VOLUME_THEORITICAL').map({
  '9.0ml in total (two 1.5ml and twelve 0.5ml)' : 9,
  '3.0ml in total' : 3,
  '4.0ml in total' : 4,
  '2.0ml in total' : 2,
  '6.0ml in total, 1.5 in each of four cryovials' : 6
  })*1000
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
//not transferred
null
```



Time on which the sample derivative was created.
---

**Variable Name  :** SAMPLE_DERIV_TIME

**Variable Label :** Time on which the source sample was aliquoted.

**Value Type     :** text

**JS script**

```{javascript}
//not transferred
null
```



Date and time on which the sample derivative was created.
---

**Variable Name  :** SAMPLE_DERIV_DATETIME

**Variable Label :** Date and time on which the source sample was aliquoted.

**Value Type     :** text

**JS script**

```{javascript}
//not transferred
null
```



Sample derivative type
---

**Variable Name  :** SAMPLE_DERIV_TYPE

**Variable Label :** Indicator of the type of sample or aliquot being referenced.

**Value Type     :** integer

**JS script**

```{javascript}
//not transferred
null
```



Sample derivative processing method (SOP)
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_METHOD_SOP

**Variable Label :** Processing method used to generate the sample derivative.Information derived from SOPs.

**Value Type     :** text

**JS script**

```{javascript}
//not transferred
null
```



DNA extraction SOP
---

**Variable Name  :** SAMPLE_DNA_EXTRACT_SOP

**Variable Label :** Name of the standard operating procedure (SOP) used to conduct DNA extration.

**Value Type     :** text

**JS script**

```{javascript}
//not transferred
null
```



DNA 260/280 ratio
---

**Variable Name  :** SAMPLE_DNA_260_280

**Variable Label :** DNA 260/280 ratio as obtain via a spectrophotometer.

**Value Type     :** decimal

**JS script**

```{javascript}
//not transferred
null
```



DNA 260/280 ratio adjusted
---

**Variable Name  :** SAMPLE_DNA_260_280_ADJ

**Variable Label :** DNA (260-320)/(280-320) ratio as obtain via a spectrophotometer.

**Value Type     :** decimal

**JS script**

```{javascript}
//not transferred
null
```



DNA concentration measured by picogreen
---

**Variable Name  :** SAMPLE_DNA_CONC_PICOGREEN

**Variable Label :** Concentration of DNA in ng/uL as measured by picogreen.

**Value Type     :** decimal

**JS script**

```{javascript}
//not transferred
null
```



DNA concentration measured by UV
---

**Variable Name  :** SAMPLE_DNA_CONC_UV

**Variable Label :** Concentration of DNA in ng/uL as measured by UV.

**Value Type     :** decimal

**JS script**

```{javascript}
//not transferred
null
```



b-globin PCR Score
---

**Variable Name  :** SAMPLE_DNA_PCR_BGLOBIN

**Variable Label :** Number of bands amplified during b-globin PCR analysis.

**Value Type     :** integer

**JS script**

```{javascript}
//not transferred
null
```



Impact on sample processing
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_FACTOR_YN

**Variable Label :** Existence of any factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//not transferred
null
```



Information critical to sample processing.
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_FACTOR

**Variable Label :** Factor that may have influenced sample processing.

**Value Type     :** integer

**JS script**

```{javascript}
//not transferred
null
```



Information critical to sample processing, other
---

**Variable Name  :** SAMPLE_DERIV_PROCESS_NOTE

**Variable Label :** Factor that may have influenced sample processing, other.

**Value Type     :** text

**JS script**

```{javascript}
//not transferred
null
```




