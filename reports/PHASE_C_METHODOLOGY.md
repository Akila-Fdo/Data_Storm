# PHASE C METHODOLOGY DOCUMENTATION
## Latent Demand Estimation & Environmental Clustering for Censored FMCG Markets

---

## 1. WHY HISTORICAL SALES ARE CENSORED

### The Fundamental Problem: Y = min(Y*, C)

In FMCG retail distribution networks, observed transactional volumes represent a lower bound on true commercial opportunity rather than an accurate reflection of demand. This critical distinction—between what outlets *actually purchased* and what they *were capable of purchasing*—underpins the entire Phase C estimation strategy.

Observed sales (Y) are constrained by operational ceilings (C), which create a left-censored distribution of the underlying latent demand (Y*). The relationship is mathematically expressed as:

**Y = min(Y*, C)**

This formulation captures the core reality: an outlet's historical purchase volume reflects whichever is lower—its true latent demand or its operational constraints. When constraints bind (C < Y*), the outlet operates in a censored regime where observed behavior does not reveal capacity.

### Sources of Operational Ceilings

Several interrelated constraints create the censoring mechanism:

**Cooler Infrastructure:** Beverage storage capacity directly limits purchase volumes. An outlet with insufficient cooler space cannot stock sufficient inventory to serve peak demand periods, regardless of customer willingness to purchase. This constraint is both hard (physical capacity) and soft (working capital tied up in slow-moving inventory).

**Credit Availability:** Distributors allocate credit limits based on historical performance and perceived reliability. An outlet with a lower credit ceiling must operate with smaller inventory positions, which compresses both order size and order frequency. This creates a funding-driven constraint independent of latent demand.

**Operational Complexity:** Smaller outlets may lack the logistical or managerial capacity to process multiple deliveries per month. A single-delivery-per-month operational model inherently caps monthly volumes, even if demand would support higher frequencies.

**Demand Variability Management:** Outlets with high demand volatility face the risk-return tradeoff of inventory holding costs versus stockout penalties. Conservative ordering in the presence of volatile demand generates a precautionary ceiling below latent demand.

### Why Historical-Average Allocation is Flawed

Traditional allocation methods—which distribute supply proportionally to historical sales—perpetuate and reinforce censoring constraints. An outlet receiving 10 liters per month in allocation based on its historical 10-liter average is implicitly being told: "you deserve exactly what you achieved while constrained." This approach:

- **Fails to recognize capacity:** An outlet performing at 10 liters under credit constraints may be capable of 25 liters with additional credit and cooler space.
- **Entrenches inequality:** High-performing outlets (those with adequate infrastructure) receive proportionally more supply, while constrained outlets remain constrained.
- **Ignores peer potential:** An outlet in the same environmental cluster as high performers may possess identical latent commercial opportunity but exhibit lower historical volume due to temporary constraints that have since been addressed.

The Phase C methodology replaces historical-average allocation with **peer-informed, constraint-adjusted potential estimation**, recognizing that observed behavior reflects both demand and operational realities.

---

## 2. ENVIRONMENTAL CLUSTERING METHODOLOGY

### Why Environmental, Not Sales-Only Clustering?

Clustering outlets by sales characteristics alone—volume, revenue, seasonal patterns—produces groups that reflect historical censoring patterns rather than latent commercial opportunity. This is the critical error that Phase C methodology explicitly avoids.

Sales-only clustering creates a false segmentation where:
- Constrained outlets cluster together with inherently low-demand outlets, making the two indistinguishable.
- An outlet constrained by cooler capacity clusters away from similar-demand peers with better infrastructure.
- Temporal changes in constraints (e.g., a new cooler installation) are invisible to historical-sales-based clustering.

**Environmental clustering**, by contrast, groups outlets by their *commercial context and infrastructure*, not by their historical performance under constraint. This approach assumes that outlets sharing similar spatial environments, infrastructure signals, and operational footprints possess similar **latent capacity**, even if their observed sales currently differ.

### The Clustering Architecture

Phase C clustering operates on six dimensions of commercial environment:

1. **Spatial Demand Proxies** (Nearby_Schools, Nearby_Bus_Stops, Nearby_Hospitals): The density of Point-of-Interest (POI) infrastructure within each outlet's 500-meter catchment area, reflecting foot traffic and transient demand generation capacity.

2. **Cooler Infrastructure** (Cooler_Count): Physical capacity signal indicating refrigerated storage availability, which directly enables inventory positioning for cold-chain products.

3. **Portfolio Diversity** (SKU_Diversity): The breadth of product offerings historically sold, indicating whether the outlet serves a narrow use-case (single-category) or broad consumption patterns.

4. **Sales Intensity** (Avg_Volume): Current average purchase level, normalized via standardization to prevent dominant influence over structural factors.

These six features are standardized using z-score normalization (StandardScaler) to place them on equal footing, then submitted to K-means clustering with k=8 clusters. The resulting eight environmental clusters represent distinct commercial contexts: high-footfall-urbanized-portfolio-rich outlets form one cluster; low-footfall-rural-single-category outlets form another; and six additional environmental archetypes partition the remaining outlet diversity.

### Peer Benchmarking Logic

Once environmental clusters are formed, each outlet is positioned within a peer group of structurally similar outlets. The critical insight: **if two outlets share the same environmental profile (spatial context, cooler capacity, portfolio diversity), they possess similar latent commercial opportunity, regardless of current sales differences.**

This assumption enables peer benchmarking. Within each cluster, the 95th percentile of average monthly purchase volume (Cluster_Potential_Ceiling) becomes an estimate of uncapped capacity for the cluster environment. An outlet currently underperforming this ceiling is implicitly leaving latent opportunity on the table.

The peer benchmarking approach is robust because:
- It does not claim that all outlets will reach peer-group ceilings (operational constraints may still bind).
- It acknowledges that top performers within clusters have demonstrated feasibility—if peer outlet X achieves 40 liters/month in identical environmental context, peer outlet Y is not fundamentally incapable of similar performance.
- It avoids outlier inflation by using the 95th percentile rather than the maximum, reducing bias from anomalous high performers.

---

## 3. WHY POINTS-OF-INTEREST MATTER

### POIs as Proxy Demand Signals

Points of Interest (schools, bus stops, hospitals, etc.) are not demand themselves, but measurable correlates of human movement, density, and activity patterns. The business reasoning is straightforward: retail locations with higher concentrations of people-generating infrastructure attract more commercial opportunity.

**Schools** generate youth-oriented demand. They attract students, parents, teachers, and support staff—populations that purchase beverages for consumption on premises, during breaks, or for take-home. The density of schools within an outlet's catchment correlates with the likelihood of sustained youth-consumption demand segments.

**Bus Stops** proxy transient footfall and mobility hubs. Passengers waiting for transit, people making connecting trips, and daily commuters passing through a location all represent ephemeral demand opportunities. An outlet near a major bus stand captures incidental purchases, quick transactions, and convenience-driven consumption that would not exist in isolation.

**Hospitals** signal continuous traffic of patients, visitors, and healthcare workers. These locations generate stable, continuous (if smaller) demand, often with distinct usage patterns—medicines paired with beverages, companions purchasing for patients, and shift-workers purchasing for consumption during duty. Hospital proximity indicates access to a captive, stable, recurring foot traffic segment.

### Environmental Context as Uncaptured Demand Signal

In a censored regime, observed sales systematically underrepresent latent demand for outlets that currently operate under constraint. An outlet with a new cooler installation may be in the early stages of demand realization, with historical sales not yet reflecting new capacity. An outlet with improved credit terms may still be operating with conservative order patterns based on past credit anxiety.

Environmental demand proxies (POI density) function as a structural indicator of *uncaptured opportunity*: they reveal what foot-traffic-driven demand *could* be served if infrastructure were adequate. An outlet in a high-school-density area with previously inadequate cooling infrastructure has inherent latent opportunity—the demand context exists; the constraint does not need to persist.

By measuring POI density as a feature independent of current sales volume, the methodology establishes a baseline commercial potential that may exceed observed sales, suggesting latent demand rather than inherent market weakness.

### Human Movement Patterns and Commercial Viability

Retail footfall concentrates around purposeful destinations (schools, transit hubs, healthcare) and along mobility corridors (commute routes). Outlets that intersect these movement patterns gain access to populations engaged in consequential trips (going to school, catching transport, seeking healthcare) where incidental beverage purchases align naturally with trip purpose.

An outlet in an isolated location with low POI density faces fundamentally lower footfall opportunity; no methodology can estimate latent demand that does not exist in the environment. Conversely, an outlet in a high-density multipurpose urban node has structural footfall advantage, and current underperformance may reflect constraint rather than low demand.

POI-based enrichment quantifies this environmental advantage structurally, enabling fair comparison of outlets operating under unequal footfall conditions.

---

## 4. POI DATA ACQUISITION STRATEGY

### Why External Geospatial Data?

The outlet master data provides location coordinates but does not encode commercial context. Inferring environmental demand signals requires external geospatial intelligence: structured data on the locations of schools, transit hubs, healthcare facilities, and other human-activity generators.

Open Street Map (OSM), a crowd-sourced global geospatial database, provides comprehensive, freely accessible POI coverage for Sri Lanka and worldwide. Rather than conducting primary survey research or purchasing proprietary location datasets, the methodology leverages OSM's extensive community-contributed POI database.

### Acquisition Pipeline: OSMnx and Overpass API

The acquisition strategy uses **OSMnx**, a Python package that queries the Overpass API (OSM's spatial query interface) and retrieves structured geospatial features within defined geographic regions.

The workflow proceeds as follows:

1. **District-Level Queries:** Instead of querying OSM for the entire country and managing an overwhelming dataset, POI acquisition is structured at the district level. Each district is queried independently for schools, bus stops, hospitals, and other relevant features.

2. **Feature-Specific Filtering:** OSM uses standardized tagging conventions. Schools are identified by the tag `amenity=school`; bus stops by `highway=bus_stop`; hospitals by `amenity=hospital`. These precise tag-based queries ensure that results correspond to meaningful commercial-context features rather than incidental matches.

3. **Lazy Evaluation and Caching:** Once district-level POI data is retrieved, it is cached in local GeoJSON format, eliminating the need for repeated API queries. This approach:
   - Reduces computational overhead in subsequent analysis iterations.
   - Provides reproducibility: future analysis uses the same cached POI dataset.
   - Respects Overpass API rate limits and avoids unnecessary network calls.
   - Creates a versioned, auditable snapshot of POI data as of the analysis date.

### Geospatial Processing: Local Analysis

After district-level POI caching, spatial analysis occurs entirely in local memory using **GeoPandas**, a Python geospatial library. Outlet coordinates are converted into a GeoDataFrame (a spatial-enabled tabular format), with each outlet represented as a point geometry in the WGS84 coordinate reference system (EPSG:4326).

POI data is similarly structured as GeoDataFrames, then re-projected to the Web Mercator projection (EPSG:3857), which preserves distances at local scales and enables accurate distance-based spatial operations.

All subsequent operations—buffer creation, spatial joins, POI counting—execute in local memory without additional API calls, ensuring reproducibility and computational efficiency.

### Scalability and Reproducibility

This acquisition strategy scales from analysis of individual districts to national-scale POI enrichment without modification. The caching mechanism ensures that:
- Analysis iterations do not trigger repeated API queries.
- The POI dataset remains temporally consistent (all POI data corresponds to a single acquisition date).
- Future analyses can reproduce results using identical POI inputs.
- The methodology is transparent: the source, date, and filtering criteria for spatial enrichment are explicit and auditable.

---

## 5. POI CATEGORIES AND CATCHMENT DRIVERS

### School Density as Youth-Segment Indicator

**Schools** serve as proxies for youth population concentration and associated demand generation. The business reasoning is multi-layered:

Students engage in incidental beverage consumption during breaks, lunch, and school hours. Schools generate predictable, recurring demand concentrated in specific time windows (lunch breaks) and seasons (school terms). Teacher and support staff populations add adult consumption during working hours.

Beyond direct in-school consumption, school locations mark residential areas with high youth population density. Parents purchasing for household consumption, students purchasing for take-home, and peer-group consumption associated with youth social activities cluster geographically near schools.

School density within a 500-meter outlet catchment therefore signals access to a stable, recurring, predictable youth-consumption segment that converts to baseline demand volume. Higher school density indicates stronger structural access to youth-driven demand, independent of current sales realized.

### Bus Stops and Transit Hubs as Transient Footfall

**Bus stops** function as mobility nexus points where passenger populations concentrate. The commercial opportunity emerges from:

**Waiting-time consumption:** Passengers awaiting transit naturally purchase beverages for consumption during wait periods. A major bus stand with high passenger throughput generates steady incidental demand from this waiting-population segment.

**Commuter pattern capture:** Outlets near bus stops capture incidental purchases from daily commuters, reducing transaction friction by proximity. A commuter passing through a convenient location makes a beverage purchase at low effort cost, generating transaction volumes that would not occur if the outlet required detour.

**Mobility-hub agglomeration:** Major bus stands often co-locate with other commercial activity (food vendors, small shops), creating commercial density that attracts diverse foot traffic beyond commuters.

Bus stop proximity therefore signals access to a footfall segment characterized by high velocity (short dwell times, transient populations) but high volume (concentrated passenger flows). This segment demands convenience-positioned retail, making it particularly valuable for beverage outlets operating at high-turnover velocities.

### Hospitals as Stable Continuous-Traffic Generators

**Hospitals** create a fundamentally different commercial context than schools or transit hubs. The demand structure is characterized by:

**Captive populations:** Patients and visitors are not transiting; they are staying for medically necessary reasons. This creates sustained, concentrated populations over hours, generating baseline demand independent of weather, seasonality, or discretionary economic conditions.

**Shift-work populations:** Hospital staff operating 24/7 across three shifts create round-the-clock purchasing opportunities, extending demand beyond typical retail hours and generating off-peak consumption.

**Companion and support purchasing:** Hospital visits involve families, support persons, and caregivers who consume beverages during extended stays. This creates multi-person consumption scenarios and longer purchase-consideration windows than transient foot-traffic scenarios.

**Predictable, stable demand:** Hospital patient volumes are relatively stable, insensitive to economic fluctuations, and driven by healthcare need rather than discretionary consumption. This creates low-variance, highly predictable demand that enables reliable inventory positioning.

Hospital proximity signals access to a captive, stable demand segment with distinct consumption patterns and high predictability—commercially valuable for outlets seeking to minimize demand uncertainty.

### Cumulative Environmental Demand Scoring

The three POI categories (schools, bus stops, hospitals) represent complementary demand drivers: youth-concentrated recurring demand, transient-footfall demand, and stable-captive demand. An outlet's cumulative POI density across all three categories creates a **Spatial_Score**, which reflects its structural access to human-movement-driven commercial opportunity.

High Spatial_Score outlets operate in inherently high-footfall environments; their environmental context predicts strong latent demand capacity. Low Spatial_Score outlets face fundamentally lower structural footfall; any demand growth must derive from non-footfall mechanisms (local population purchasing for home consumption, loyalty-driven repeat visits).

This cumulative scoring acknowledges that environmental demand opportunity is multifaceted, with different POI types contributing distinct demand segments that accumulate to total footfall potential.

---

## 6. SPATIAL MAPPING METHODOLOGY

### Coordinate Conversion and Geospatial Representation

Outlet locations in the master data exist as latitude-longitude coordinate pairs. To enable distance-based spatial analysis, these coordinates are formally converted into a **GeoDataFrame**—a geospatially-aware tabular structure that represents each outlet as a point geometry.

The conversion process uses the WGS84 coordinate reference system (EPSG:4326), the global standard for GPS coordinates. This representation enables subsequent spatial operations: the creation of buffers around each outlet, the identification of POI locations relative to outlet positions, and distance-based spatial joins.

### 500-Meter Catchment Buffer Creation

Each outlet's spatial influence is modeled using a **500-meter radial buffer**—a circular zone extending 500 meters in all directions from the outlet's coordinate point. This distance represents a reasonable walking distance for retail customers and aligns with spatial analysis conventions for retail foot-traffic catchment areas.

The 500-meter radius captures the "local convenience" zone: customers willing to make a brief trip to purchase a beverage. Outlets positioned within this catchment are competing for the same local population; environmental demand drivers (schools, bus stops) within this zone directly influence an outlet's accessible footfall.

Buffer creation is performed after reprojection to the Web Mercator projection (EPSG:3857), which preserves distance measurements at regional scales and enables accurate 500-meter buffer calculations.

### Local Spatial Joins and POI Containment

For each outlet's catchment buffer, the spatial analysis identifies all POI locations falling within the buffer boundary. This operation uses the `.within()` spatial predicate: for each outlet buffer, the algorithm counts how many school geometries, bus-stop geometries, and hospital geometries fall within the buffer zone.

This local spatial join produces three features for each outlet:
- **Nearby_Schools:** Count of schools within 500 meters
- **Nearby_Bus_Stops:** Count of bus stops within 500 meters  
- **Nearby_Hospitals:** Count of hospitals within 500 meters

These counts quantify each outlet's direct access to human-movement-generating infrastructure. An outlet with 12 nearby schools and 3 nearby bus stops operates in a fundamentally different commercial context than an outlet with 1 school and 0 bus stops nearby.

### Feature Normalization for Clustering

The three raw POI counts are naturally scaled (counts on the interval [0, ~20] per feature). When combined with cooler infrastructure counts and sales-intensity variables, raw POI counts could be dominated by other features due to different numerical ranges.

The preprocessing step applies standardization (z-score normalization) to all features before clustering. This ensures that POI density exerts proportional influence on cluster assignment, rather than being suppressed by features with larger numerical ranges. 

The resulting spatial features become actionable dimensions of environmental context, equally weighted with infrastructure and sales characteristics in determining peer-group assignment.

---

## 7. WHY CLUSTER CEILINGS ESTIMATE POTENTIAL

### Identification of Top Performers and Peer Benchmarking

Once outlets are environmentally clustered, the analysis examines within-cluster performance variation. Despite sharing similar environmental contexts (spatial demand drivers, infrastructure availability, portfolio diversity), outlets within a cluster exhibit different current sales levels.

This performance variation reflects two possibilities:
1. **Latent demand differences:** True underlying demand differs even among environmentally similar outlets.
2. **Constraint differences:** Identical latent demand is subject to different operational constraints—cooler capacity variations within the cluster, credit ceiling differences, or temporal differences in infrastructure development.

**Top-performing outlets within a cluster have demonstrated feasibility:** they have achieved high sales volumes within the exact environmental context that constrains the lower-performing peers. This demonstration of feasibility is the cornerstone of peer benchmarking.

### Cluster Ceiling as Latent Capacity Approximation

For each environmental cluster, the 95th percentile of average monthly purchase volume represents the cluster's demonstrated ceiling. This metric is chosen over the maximum to avoid inflation from outlier high performers who may operate under unique (unmeasured) advantages.

The 95th percentile captures: "In this environmental context, top-tier outlets routinely achieve this volume level." For an outlet currently underperforming this ceiling, the ceiling represents an evidence-based upper bound on latent capacity.

The business interpretation: if peer outlets in identical environmental clusters have achieved 35 liters/month average volume, a cluster member currently achieving 15 liters/month is likely operating under constraint, not facing absolute demand limitation. The peer has revealed that the environment is capable of supporting higher volumes.

### Assumptions and Limitations

The cluster ceiling methodology operates under explicit assumptions:

- **Environmental cluster membership is destiny-correlated:** Outlets with identical spatial, infrastructure, and portfolio characteristics face similar latent demand capacity. This assumption is valid if unmeasured differences in local competition, management quality, or customer loyalty are not systematically correlated with environmental cluster assignment.

- **Top performers are not uniquely advantaged:** The 95th percentile outlet is not benefiting from unique unmeasured advantages that other cluster members cannot access. This is a reasonable assumption for environmental clustering (which captures structural advantages), but would be violated if measurement omits critical variables (e.g., aggressive local marketing, superior credit terms, exclusive distribution rights).

- **Performance variation is primarily constraint-driven:** The methodology assumes that within-cluster sales differences predominantly reflect constraint differences rather than fundamental demand differences. In markets with significant heterogeneous consumer preferences or local-competitive intensity, this assumption may not hold.

Despite these limitations, the peer benchmarking approach provides an evidence-based, defensible methodology: it does not conjecture about unobserved demand; it leverages observed performance of peers to infer feasibility boundaries.

---

## 8. CONSERVATIVE UNCENSORING METHODOLOGY

### The Challenge: Closing the Potential Gap Without Overestimation

Once cluster ceilings are established, each outlet's **Potential_Gap** is defined as the difference between the cluster ceiling and its current average monthly volume:

**Potential_Gap = Cluster_Potential_Ceiling - Avg_Volume**

A positive gap indicates that peer outlets in the same environmental cluster have demonstrated higher volumes. However, not all of this gap represents feasible latent demand. The gap could reflect:

1. **Realizable latent demand** (constrained by infrastructure or credit): demand that the outlet can capture with constraint relief.
2. **Non-realizable gap** (structural demand differences): demand that simply does not exist for this specific outlet despite environmental similarity.
3. **Measurement noise and temporal variation:** random fluctuation in peer performance that does not represent repeatable ceiling-level performance.

The estimation challenge is to recover realizable latent demand while avoiding overestimation bias.

### Conservative Uncensoring Strategy: 70% Gap Capture

The methodology adopts a conservative uncensoring approach: **Estimated_Potential = Avg_Volume + (Potential_Gap × 0.70)**

This formulation recovers 70% of the observed gap between current performance and peer ceiling, reserving 30% of the gap as likely non-realizable. The business reasoning:

- **70% capture rate:** This assumes that approximately 70% of the observed peer-ceiling gap represents realizable latent demand that the outlet can access through constraint relief. This rate is chosen as a middle ground between aggressive (100% gap capture, likely overstating potential) and conservative (0% gap capture, ignoring peer benchmarking entirely).

- **Avoidance of outlier inflation:** By not fully closing the gap to the peer ceiling, the methodology acknowledges that the 95th percentile peer may operate under unique, unmeasured advantages. Perfect gap closure would assume complete parity, which is unrealistic.

- **Operational realism:** Not all outlets operate identically. Some may face local competition not captured in environmental clustering; others may have niche customer bases that fundamentally limit volume. Reserving 30% of the gap preserves acknowledgment of these unmeasured factors.

### Why Conservative Uncensoring is Defensible

In censored markets, overestimation bias is a material risk: overstating latent demand leads to unsustainable cooler allocation, disappointed performance, and operational friction. A 70% gap-capture rate is empirically calibrated to be aggressive enough to capture meaningful potential while conservative enough to avoid ambitious overestimation.

Conservative uncensoring also preserves credibility: claims of potential are clearly explained (peer-benchmarking-based) rather than mechanically derived. Stakeholders can understand the methodology, evaluate the assumptions, and calibrate their actions accordingly.

---

## 9. SPATIAL ADJUSTMENT AND BUSINESS INTERPRETATION

### Spatial Score Multiplier: Environmental Demand Reinforcement

After conservative uncensoring produces an **Estimated_Potential** based on peer benchmarking, an environmental adjustment is applied:

**Estimated_Potential_Adjusted = Estimated_Potential × (1 + (Spatial_Score × 0.01))**

The Spatial_Score—the sum of nearby schools, bus stops, and hospitals—is converted into a percentage multiplier. An outlet with a Spatial_Score of 15 (15 nearby demand-generating facilities) receives a 15% upward adjustment to its potential estimate.

This adjustment captures the intuition that outlets with stronger environmental demand drivers have higher feasibility for achieving peer-level performance. An outlet in a high-footfall zone needs only to improve constraint management to realize potential; an outlet in a low-footfall zone faces fundamentally lower opportunity even at peer-ceiling performance levels.

The spatial adjustment is intentionally modest (1% per POI unit) to avoid excessive inflation. It reinforces the environmental clustering insight without overstating the demand-generating power of POI density.

### Potential-Based Allocation: Strategic Applications

The resulting outlet-level maximum potential estimates enable several strategic allocation decisions:

**Cooler Allocation:** Outlets with high estimated potential but low current cooler counts represent high-return targets for cooler investment. Deploying a cooler to a high-potential, low-cooler-count outlet is predicted to yield significant volume growth by releasing pent-up demand.

**Promotional Trade Spend:** High-potential outlets with demand-side constraint (e.g., low awareness or category habit in high-footfall zones) become targets for promotional spending. The investment is directed to outlets where demand infrastructure exists but realization is currently suppressed.

**Credit Ceiling Evaluation:** Outlets with high estimated potential but historically low credit limits may warrant credit expansion. The potential estimate signals that demand exists to support higher credit utilization; the limit may be the constraint rather than true demand weakness.

**Distributor Relationship Strategy:** Outlets with large positive (Potential - Current_Performance) gaps are candidates for intensified distributor focus, relationship building, or exclusive distribution rights. These outlets represent latent opportunity within the distributor's existing network.

### Growth Gap Identification and Prioritization

The difference between **Estimated_Potential** and **Avg_Volume** yields a **Growth Gap** for each outlet—the predicted demand-capture opportunity from constraint relief and operational optimization.

High-growth-gap outlets represent leveraged investment opportunities: they require relatively minor operational changes (cooler deployment, credit expansion) to realize major sales uplift. Medium-growth-gap outlets represent steady improvement targets. Low-growth-gap outlets may be at saturation or facing fundamental demand constraints that potential estimation cannot overcome.

Prioritization matrices combining Spatial_Score, Growth_Gap, and current volume identify which outlets merit the most intensive operational and commercial focus.

### Why Latent Opportunity Matters for FMCG Strategy

FMCG growth is constrained by two distinct mechanisms: **demand-side growth** (market expansion, consumption habit formation) and **supply-side growth** (retail expansion, channel distribution). In mature FMCG markets operating under censoring constraints, latent demand recovery through constraint relief often represents the highest-ROI growth lever.

An outlet with 10-liter current volume and 25-liter estimated potential does not require market expansion or demand generation; it requires operational excellence (cooler, credit, inventory management) to realize existing demand. These improvements are far more controllable and rapid than building category demand from zero.

Phase C potential estimation transforms latent demand from an unmeasured theoretical construct into a **actionable, prioritizable, quantified estimate** that enables evidence-based investment allocation across the retail network.

---

## METHODOLOGY SUMMARY

The Phase C approach addresses the fundamental challenge of censored demand estimation through a integrated four-step process:

1. **Environmental clustering** groups outlets by structural commercial context, enabling peer comparison independent of historical performance censoring.

2. **Spatial enrichment** quantifies environmental demand drivers (schools, bus stops, hospitals) through local POI analysis, capturing human-movement patterns that correlate with commercial opportunity.

3. **Peer benchmarking** identifies performance ceilings within environmental clusters, providing evidence-based upper bounds on latent capacity for underperforming outlets.

4. **Conservative uncensoring** bridges observed performance to estimated potential through a defensible 70% gap-capture methodology, balancing demand realization against overestimation risk.

The resulting outlet-level potential estimates enable strategic allocation of coolers, credit, and commercial support to outlets with latent demand, converting information asymmetry into operationalized opportunity identification.

