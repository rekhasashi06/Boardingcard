# Boardingcard
Boardingcard sorting program
class BoardingCard:
    def __init__(self, departure, destination, transport_type, seat_assignment=None, other_details=None):
        self.departure = departure
        self.destination = destination
        self.transport_type = transport_type
        self.seat_assignment = seat_assignment
        self.other_details = other_details

    def __str__(self):
        details = f'Take {self.transport_type} from {self.departure} to {self.destination}.'
        if self.seat_assignment:
            details += f' Sit in seat {self.seat_assignment}.'
        if self.other_details:
            details += f' {self.other_details}'
        return details


def parse_boarding_cards(input_data):
    boarding_cards = []
    for i, card_info in enumerate(input_data):
        departure = card_info.get('departure')
        destination = card_info.get('destination')
        transport_type = card_info.get('transport_type')
        seat_assignment = card_info.get('seat_assignment')

        # Create a boarding card with the correct departure and destination
        boarding_card = BoardingCard(
            departure=departure,
            destination=destination,
            transport_type=transport_type,
            seat_assignment=seat_assignment
        )

        boarding_cards.append(boarding_card)

    return boarding_cards


def sort_boarding_cards(boarding_cards):
    # Create a mapping of departure -> card
    departure_to_card = {card.departure: card for card in boarding_cards}
    destination_to_card = {card.destination: card for card in boarding_cards}

    # Find the starting point (departure that is not a destination)
    start_card = None
    for card in boarding_cards:
        if card.departure not in destination_to_card:
            start_card = card
            break

    sorted_cards = [start_card]

    # Build the sorted journey
    while len(sorted_cards) < len(boarding_cards):
        destination = sorted_cards[-1].destination
        next_card = departure_to_card[destination]
        sorted_cards.append(next_card)

    return sorted_cards


def generate_journey_description(sorted_boarding_cards):
    journey_description = []
    for i, card in enumerate(sorted_boarding_cards):
        description = f"{i + 1}. {str(card)}"
        journey_description.append(description)
    journey_description.append("You have arrived at your final destination.")
    return journey_description


def main(input_data):
    boarding_cards = parse_boarding_cards(input_data)
    sorted_boarding_cards = sort_boarding_cards(boarding_cards)
    journey_description = generate_journey_description(sorted_boarding_cards)
    return journey_description


if __name__ == "__main__":
    input_data = [
        {"departure": "Madrid", "destination": "Barcelona", "transport_type": "train 78A", "seat_assignment": "45B"},
        {"departure": "Barcelona", "destination": "Gerona Airport", "transport_type": "airport bus"},
        {"departure": "Gerona Airport", "destination": "Stockholm", "transport_type": "flight SK455", "seat_assignment": "3A", "baggage_drop": "ticket counter 344"},
        {"departure": "Stockholm", "destination": "New York JFK", "transport_type": "flight SK22", "seat_assignment": "7B", "baggage_transfer": True}
    ]

    journey_description = main(input_data)

    # Print the journey description
    for step in journey_description:
        print(step)
