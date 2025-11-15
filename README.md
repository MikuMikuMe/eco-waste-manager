# eco-waste-manager

Creating a smart waste management system leveraging IoT and data analytics involves several components, including data collection from IoT devices, data processing to optimize routes, and scheduling. Given that we don't have an actual IoT setup here, I'll provide a simulated version of this system, focusing on key aspects. This example assumes you've got IoT sensors that simulate waste levels in different bins across a city.

```python
import random
import time
import logging
from datetime import datetime, timedelta
from collections import namedtuple

# Setup logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# Simulate some IoT devices gathering data (e.g., waste bin sensors)
WasteBin = namedtuple('WasteBin', ['id', 'location', 'capacity', 'current_level'])

# Function to simulate waste level in bins
def generate_waste_data(number_of_bins=10):
    bins = []
    for i in range(number_of_bins):
        bin_id = f"BIN_{i+1}"
        location = f"Location_{i+1}"
        capacity = random.randint(100, 200)  # Assume capacity in liters
        current_level = random.randint(0, capacity)
        bins.append(WasteBin(bin_id, location, capacity, current_level))
    return bins

# A simple function to calculate fill percentage
def calculate_fill_percentage(waste_bin):
    return (waste_bin.current_level / waste_bin.capacity) * 100

# Function to determine optimized collection schedule
def optimize_collection_schedule(bins):
    # Sort bins by fill percentage descending
    sorted_bins = sorted(bins, key=calculate_fill_percentage, reverse=True)
    # Assume collection is done for bins more than 75% filled
    bins_to_collect = [b for b in sorted_bins if calculate_fill_percentage(b) > 75]
    return bins_to_collect

# Dummy route optimization logic. In real-life, this would require GIS data and algorithms
def optimize_collection_routes(bins_to_collect):
    # For simplicity, just return the order of bins to be collected
    routes = [bin.location for bin in bins_to_collect]
    return routes

# Simple function to schedule collections
def schedule_collections():
    try:
        # Generate sample data
        bins = generate_waste_data()

        # Optimize collection schedule
        bins_to_collect = optimize_collection_schedule(bins)

        # Output bin statuses
        for bin in bins:
            logging.info(f"{bin.id} at {bin.location}: {calculate_fill_percentage(bin):.2f}% full")

        # Get optimized routes
        routes = optimize_collection_routes(bins_to_collect)

        # Scheduling collections
        if bins_to_collect:
            logging.info("Bins scheduled for collection:")
            for bin in bins_to_collect:
                logging.info(f"{bin.id} at {bin.location} ({calculate_fill_percentage(bin):.2f}% full)")

            logging.info("Optimized collection route: " + " -> ".join(routes))
        else:
            logging.info("No bins require collection at this time.")

    except Exception as e:
        logging.error(f"An error occurred during scheduling: {str(e)}")

# Simulate periodic scheduling, e.g., daily
if __name__ == "__main__":
    while True:
        logging.info("Starting waste collection scheduling...")
        schedule_collections()
        # Run the schedule task every 24 hours
        time.sleep(24 * 3600)  # 24 hours
```

### Overview of the Program:

- **Data Generation**: The `generate_waste_data` function simulates data collection from IoT sensors on bins, including their current waste levels.
  
- **Optimization**:
  - **Fill Calculation**: The `calculate_fill_percentage` calculates how full each bin is.
  - **Schedule Optimization**: The `optimize_collection_schedule` decides which bins need immediate collection based on a threshold fill level (e.g., 75%).
  - **Route Optimization**: The `optimize_collection_routes` determines the order in which the bins should be collected, which would ideally be based on GIS data for optimal routing.

- **Scheduling**: The waste collection scheduling is logged, and the optimized routes and bins needing collection are reported.

- **Execution Loop**: The script is designed to run indefinitely with a 24-hour interval between each scheduling attempt, which is configurable based on real-time needs.

- **Error Handling**: All major sections are wrapped in try-except blocks to print out any errors that might occur during data processing, optimizing, or scheduling.