def main():
    while True:
        # Get the route number from the user.
        route_number = input("Please enter the route number: ")
        
        # Validate route number as a positive integer.
        if not route_number.isdigit() or int(route_number) <= 0:
            print("Invalid route number. Please enter a positive integer.")
            continue

        # Get the number of stops on the route from the user.
        try:
            stops_number = int(input("Please enter the number of stops on this route: "))
        except ValueError:
            # Print an error message if the user enters an invalid input.
            print("Invalid input. Please enter a positive integer.")
            continue

        # Check if there are at least three stops on the route.
        if stops_number < 3:
            print("Invalid number of stops. There must be at least three stops on the route.")
            continue

        # Get the number of passengers at the start of the route.
        try:
            passengers_start = int(input("How many passengers were waiting for the bus at stop #1? "))
        except ValueError:
            # Print an error message if the user enters an invalid input.
            print("Invalid input. Please enter a non-negative integer.")
            continue

        # Initialize variables to keep track of passenger counts.
        total_happy_customers = passengers_start
        total_unhappy_customers = 0
        passanger_on_bus = {1: passengers_start}

        # Iterate over the stops from 2 to the number of stops.
        for stop in range(2, stops_number + 1):
            # Get the number of passengers who left the bus at this stop.
            try:
                passangers_leaving = int(input(f"How many passengers left the bus at stop #{stop}? "))
            except ValueError:
                # Print an error message if the user enters an invalid input.
                print("Invalid input. Please enter a non-negative integer.")
                continue

            # Ensure that passengers leaving do not exceed passengers on the bus.
            if passangers_leaving > passanger_on_bus[stop - 1]:
                print(f"Invalid number of passengers. Only {passanger_on_bus[stop - 1]} passengers were on the bus.")
                continue

            # Get the number of passengers who were waiting for the bus at this stop.
            try:
                passangers_waiting = int(input(f"How many passengers were waiting for the bus at stop #{stop}? "))
            except ValueError:
                # Print an error message if the user enters an invalid input.
                print("Invalid input. Please enter a non-negative integer.")
                continue

            # Calculate the number of passengers on the bus at this stop.
            passanger_on_bus[stop] = passanger_on_bus[stop - 1] - passangers_leaving
            passanger_on_bus[stop] = min(35, passanger_on_bus[stop] + passangers_waiting)

            # Update the total number of happy and unhappy customers.
            total_happy_customers += passangers_waiting
            total_unhappy_customers += max(0, passangers_waiting - (35 - passanger_on_bus[stop]))

        # Calculate the ratio of happy to unhappy customers.
        ratio = total_happy_customers / (total_unhappy_customers + 1) if total_unhappy_customers > 0 else 0

        # Print the route number, number of happy customers, number of unhappy customers, and ratio of happy to unhappy customers.
        print(f"Route number: {route_number}")
        print(f"Happy customers: {total_happy_customers}")
        print(f"Unhappy customers: {total_unhappy_customers}")
        print(f"Ratio of happy to unhappy customers: {ratio:.2f}")

        another_route = input("Do you want to enter another route? (yes/no): ").lower()
        if another_route != "yes":
            print("Thank you for using the bus calculator!")
            break

if __name__ == "__main__":
    main()

