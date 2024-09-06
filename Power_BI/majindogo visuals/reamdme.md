
Maji Ndogo Water Services Data Project - README
Project Overview
This Power BI project focuses on visualizing and analyzing water access data in Maji Ndogo. The dataset integrates information from water source surveys, pollution data, queue compositions, and crime reports, with the goal of identifying patterns and challenges related to water access. The project leverages Power BI’s capabilities to explore key relationships across various dimensions like location, water source type, and demographic splits.

Key Features
1. Data Model
Multi-Star Schema: The model integrates several tables, including location, water_source, visits, well_pollution, and queue_composition. These provide a fact-dimension structure to analyze various aspects of water access.
Complex Relationships: Issues related to many-to-many relationships were addressed by leveraging unique identifiers or creating bridge tables.
Bi-Directional Filtering: Applied selectively to enhance interactivity in visualizations, particularly between water_source and visits tables.
2. Visualizations
Map of Provinces: Displays the geographical spread of water sources across Maji Ndogo, filtered by province and towns. This is used to highlight the total number of water sources and types in each region.
Population Splits: A pie/doughnut chart showing the rural vs urban population split based on water source availability.
Water Source Tree Map: Illustrates the total number of people using each water source type, helping stakeholders understand the reliance on different water sources.
Crime and Water Sources: A focused report showing crime incidents related to water sources, which helps identify vulnerable groups (e.g., women and children) during water collection times.
3. Queue Visualizations
Queue Composition by Hour: A line plot showing average queue times, segmented by hours of the day, which provides insights into peak collection times.
Gender and Queue Analysis: Visualizes the composition of queues by gender, giving insight into how men, women, and children are affected by water collection times.
Insights
1. Water Source Accessibility
High Urban Dependency: Urban populations tend to have better access to improved water sources, while rural populations rely more on shared sources.
Source Type Distribution: The majority of people use shared taps, wells, and boreholes. However, improved access is unevenly distributed, with some towns showing significant deficits in clean water availability.
2. Queue Composition
Gender Disparity: Women and children are often overrepresented in queue compositions, particularly during early mornings and evenings. Men are more commonly seen in queues during midday.
Peak Queue Times: The busiest queue times are around 15:00-16:00, when most people collect water, particularly from shared taps.
3. Pollution and Crime
Polluted Water Sources: Significant portions of the water sources tested showed signs of biological or chemical contamination, with Akatsi being particularly affected by chemical pollution.
Crime at Water Sources: Women are disproportionately victims of crimes at water sources, particularly harassment and sexual assault. The data reveals that crime rates spike on weekends and in the early morning or late evening, when women and children are most vulnerable
