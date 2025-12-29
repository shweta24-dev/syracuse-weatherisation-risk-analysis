**Title: Analysing Weatherisation Risk across Syracuse Neighbourhoods:**

**Project Overview:**

Syracuse experiences long and severe winters, with cold weather conditions lasting for a significant portion of the year. For many residents, especially those who are living in the older or rental housing, inadequate heating and poor building insulation can pose serious health and safety risk during winter months. 

The goal of the project is to identify Syracuse neighbourhoods that are most vulnerable to the **weatherisation risk** - defined as the **likelihood of facing heating related issues** during winter months. By analyzing historical heating code violations, currently unresolved heating risks, rental compliance, and neighborhood-level socioeconomic indicators, this project aims to highlight areas where residents may be disproportionately exposed to unsafe or unreliable heating conditions.

The **primary objective** is to support data-driven prioritization for the City of Syracuse by identifying neighborhoods that may benefit most from targeted weatherisation programs, inspections, or policy interventions. A **secondary objective** is to provide residents with greater visibility into the relative heating-related risk within their neighborhoods.

The key stakeholders for this analysis are city policymakers and housing officials, who can use these insights to allocate resources more effectively, and Syracuse residents, who may use this information to better understand heating-related risks in their communities.

**Key Metrics used for Risk Prioritisation**

To assess weatherszation risk across Syracuse neighborhoods, this project integrates three complementary layers of data: heating system reliability, rental compliance, and neighborhood vulnerability indicators.

**1.Heating Violation Metrics (Primary Risk Signal):** Heating-related code violations serve as the core indicator of weatherization risk.

a. Space heating system and water heating system violations
b. Total heating violations per neighborhood 
c. Open vs. closed violations, capturing both: Historical risk (past violations) and Current risk (open, unresolved violations)

📅 Violation data spans from 2017 to 2025, enabling both historical and current risk assessment.

**2. Rental Compliance Metrics (Housing Condition & Accountability):** Rental compliance data provides insight into the structural and regulatory condition of rental housing stock.

a. Total rental properties per neighborhood ( Rental Concentration)
b. Invalid rental registrations
c. Rental risk score, combining: Rental concentration and Rental non-compliance rate

This layer focuses only on rental properties, reflecting housing conditions for residents most dependent on landlord-maintained heating systems.

**3. Socioeconomic Vulnerability Indicators (Census Data):** Census-derived variables capture the capacity of residents to absorb heating failures and respond to winter conditions.

**I) Economic indicators:**  Median household income, Poverty rate (% of individuals below poverty line), SNAP participation rate (% of households receiving food assistance).




***Data Structure***

erDiagram

  CODE_VIOLATIONS {
    string SBL
    string Neighborhood
    string ViolationType
    string Status
    date ViolationDate
  }

  RENTAL_REGISTRY {
    string SBL
    string NeedsRR
    string RRisValid
  }

  SBL_NEIGHBORHOOD_MAP {
    string SBL
    string Neighborhood
  }

  NEIGHBORHOOD_HEATING_METRICS {
    string Neighborhood
    int total_heating_violations
    int open_violations
    float open_violation_pct
    float violations_per_address
  }

  NEIGHBORHOOD_RENTAL_COMPLIANCE {
    string Neighborhood
    int rental_properties
    int invalid_rentals
    float rr_invalid_rate
    float rental_risk_score
  }

  CENSUS_DP03 {
    string GEOID
    float median_household_income
    float poverty_rate_pct
    float snap_households_pct
  }

  CENSUS_DP04 {
    string GEOID
    float pct_housing_pre_1980
    float pct_housing_pre_1960
  }

  TRACTS_GEOMETRY {
    string GEOID
    geometry geom
  }

  NEIGHBORHOODS_GEOMETRY {
    string Neighborhood
    geometry geom
  }

  TRACT_NEIGHBORHOOD_MAP {
    string GEOID
    string Neighborhood
  }

  NEIGHBORHOOD_CENSUS_FEATURES {
    string Neighborhood
    int neighborhood_population
    float median_household_income
    float poverty_rate_pct
    float snap_households_pct
    float pct_housing_pre_1980
    float pct_housing_pre_1960
  }

  WEATHERIZATION_FULL {
    string Neighborhood
  }

  CODE_VIOLATIONS ||--o{ SBL_NEIGHBORHOOD_MAP : "derive SBL->Neighborhood"
  RENTAL_REGISTRY ||--|| SBL_NEIGHBORHOOD_MAP : "join on SBL"

  CODE_VIOLATIONS ||--o{ NEIGHBORHOOD_HEATING_METRICS : "aggregate by Neighborhood"
  SBL_NEIGHBORHOOD_MAP ||--o{ NEIGHBORHOOD_RENTAL_COMPLIANCE : "aggregate rentals by Neighborhood"

  CENSUS_DP03 ||--|| TRACTS_GEOMETRY : "join on GEOID"
  CENSUS_DP04 ||--|| TRACTS_GEOMETRY : "join on GEOID"
  TRACTS_GEOMETRY ||--o{ TRACT_NEIGHBORHOOD_MAP : "spatial assignment"
  NEIGHBORHOODS_GEOMETRY ||--o{ TRACT_NEIGHBORHOOD_MAP : "spatial assignment"

  TRACT_NEIGHBORHOOD_MAP ||--o{ NEIGHBORHOOD_CENSUS_FEATURES : "aggregate tracts->Neighborhood"

  NEIGHBORHOOD_HEATING_METRICS ||--|| WEATHERIZATION_FULL : "join on Neighborhood"
  NEIGHBORHOOD_RENTAL_COMPLIANCE ||--|| WEATHERIZATION_FULL : "join on Neighborhood"
  NEIGHBORHOOD_CENSUS_FEATURES ||--|| WEATHERIZATION_FULL : "join on Neighborhood"


**II) Structural housing indicators:** Percentage of housing built before 1980, Percentage of housing built before 1960.





