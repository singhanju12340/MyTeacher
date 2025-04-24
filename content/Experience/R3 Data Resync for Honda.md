---
Creation Time: Monday, September 16th 2024
Modified Time: Monday, April 14th 2025
---
https://tekion.atlassian.net/wiki/spaces/DRP/pages/3868656250/R3+Rearchitecture

https://tekion.atlassian.net/wiki/spaces/DRP/pages/3351544810/Event+based+R3+Update

ModelCode(M), LenderCode(L), Condition(C), and CertificationTier(C) , mileage 
1 vin 1 r3 calls


### Steps 1 

STAR:

Situation:
Honda Inventory Ingestion, for program Honda new vehicles, where pricing details needs to be added in discovery index along with other vehicle details, and pricing information was available from other team via cross cluster API calls.
Vin count Honda New: 15K 

Task:
1. Pricing needs to be updated for all the existing vins by daily sync jobs.
2. If new vins are getting ingested, add pricing data from cache if exist else make async process calls to add pricing data in existing half cooked vin inventory details and publish vin as available vin.
3. Pricing details was common for given models

Action:

3. Total max model count: 50 
4. For each model pricing update if any update all associated vins
50 Pricing API calls to pricing team
fetched associated vins for given model. 15K/ 50

Result
1. New vin resync was done eaisly within ms via async process
2. Complete inventory resync worked in 15 min max for 15K vins.


### Step 2
Situation
Honda's new program onboarded. 
For Acura CPO vehicles
 Here also pricing details needs to be added in discovery index along with other vehicle details, and pricing information was available from other team via cross cluster API calls.
Vin count Acura CPO: 5K 
`Problem:` Later Acura CPO vin count increased to 15K and Acura New Vehicles onboarded with an estimates of 40/50K vins
5K Acura CPO vins: 5k API calls initially
 Later it increased to 15K + 50K API calls and resync time increased to 10 hours because api calls was sync and allowed only sequencal calls. Also pricing api started responding slower with more request calls, this further lead to cascading api timeouts and retries.
 
65K calls/30 within a span of mins = 2.5k / min 
API response increased from 100ms to 30 sec and it started timeout and retries started happening and it added cascading effects to entire service sync the retry was handled async it eventully whole resync process which was suppoed to complete in 1 hr started taking 10 -15 hours and some times more.


Task:
1. Pricing needs to be updated for all the existing vins by daily sync jobs.
2. If new vins are getting ingested, add pricing data from cache if exist else make async process calls to add pricing data in existing half cooked vin inventory details and publish vin as available vin.
3. Pricing details was not based on modelIds, if was uniquely based on  `ModeCode(M), LenderCode(L), Condition(C), and CertificationTier(C) , mileage 
Action:
> CPO vehicles 5 prising request params combinations did not have common values. 
	so almost 15K calls was needed for Acura CPO.
> Acura new vehicles **milage params was almost constant values**, so we found there was almost 60/80 vins associated with prising unique params
combinations: `ModeCode(M), LenderCode(L), Condition(C), and CertificationTier(C)
>Reartchictured  Resync logic to fetch all vins and aggregate unique combinations of `ModeCode(M), LenderCode(L), Condition(C), and CertificationTier(C).` then call pricing API and update associated vins in a sync ways.
60/50K calls for Acura New vins pricing details fetching.

Result
Batch processing reduced sync api calls and further reduced complete resync time and complete inventory resync for Acura new 50K vins was again processed in 1.5 hours.

### Step 3
Situation: 
Acura new Vin count is expected to be increased by 1/2 lakhs
It will have unique model: 100 max
Task:
Resync all in max 1 hours 
Actions:
1. Other 4 params based on that Pricing API response changes needs to be taken care as pre-calculations measures so that CX only needs to make max 100 fetch and rest calculations can be done at CX based on the pre-definded rules. as these pricing rules changes are very rare.
2. Pricing team can upload calculatied data or calculation formula based on per model in cloud.
3. CX will fetch model pricing data and model pricing rule, apply it calcualte the final pricing details and sync it in all associated vins.
We was planning to fetch the latest model data(the large superset data from the blob, do the comparision from the existing model data by comparing hash of the 2 files). if there is any difference then only we will apply the calculations and apply those changes on the associated vins. 
result:
It will result in minimal resource utilisation, less computation and faster resync of complete inventory data. now resync resulted in 30 min with 2 instances compared to previously it was 10 instances in 10-15 hours. 
It also made solution extensible with increasing inventory and clients different new vehicles expesion at our CX platform










