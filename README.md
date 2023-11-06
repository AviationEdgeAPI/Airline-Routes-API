# Airline-Routes-API
[Aviation Edge Airline Routes API](https://aviation-edge.com/airline-routes-database-and-api/) provides static data on routes of operating airlines. The API focuses on general information that does not require real-time updates, so this type of details such as flight times, dates, delays, cancellations, are not included in this API. If you are looking for these, please see our [Real-time Airport Schedules API](https://github.com/AviationEdgeAPI/Airport-Schedules-API) instead or contact us below for the details.

The API has global coverage with the exception of military, private and other non-scheduled routes. When implemented, the users can view the possible flight options from airport A to airport B. This can be filtered further based on airlines.

### Documentation
You may find input parameters, output examples with explanations for each item, filter list, and more in the [documentation](https://aviation-edge.com/developers/).

### Example Fields of Use
- Building virtual maps of destinations or airlines for the existing routes they have
- Travel planning services where users are expected to submit a departure and/or arrival airport and view available flights
- Airway and route traffic analysis
- Airline comparison for available routes

### Request 
Static data on routes related to specific airports, airlines or flights:

**GET** `https://aviation-edge.com/v2/public/routes?key=[API_KEY]&departureIata=IAH`

**GET** `https://aviation-edge.com/v2/public/routes?key=[API_KEY]&departureIcao=KIAH`

**GET** `https://aviation-edge.com/v2/public/routes?key=[API_KEY]&airlineIata=AA`

**GET** `https://aviation-edge.com/v2/public/routes?key=[API_KEY]&airlineIcao=AAL`

**GET** `https://aviation-edge.com/v2/public/routes?key=[API_KEY]&flightNumber=2668`

Data on a specific route:

**GET** `https://aviation-edge.com/v2/public/routes?key=[API_KEY]&departureIata=IAH&departureIcao=KIAH&airlineIata=AA&airlineIcao=AAL&flightNumber=2668`

(only one code is enough per data -either the IATA or the ICAO code-, both are not obligatory.)

### Useful Code Examples

1.	Fetching the Data With the API

```
const axios = require('axios');

const API_KEY = 'YOUR_API_KEY';  // Replace with your actual API key
const endpointUrl = `https://aviation-edge.com/v2/public/routes?key=${API_KEY}&departureIata=OTP`;

async function fetchData() {
    try {
        const response = await axios.get(endpointUrl);
        return response.data;
    } catch (error) {
        console.error('Error fetching data:', error);
    }
}

fetchData().then(data => {
    console.log(data);
});
```

2.	Filtering routes

When you have multiple routes in your response, you might want to filter routes by a specific arrival airport (please see “Request” section for other available parameters). Here's a snippet to achieve that:

```
function filterByArrivalIata(data, arrivalIata) {
    return data.filter(route => route.arrivalIata === arrivalIata);
}

// Example usage
fetchData().then(data => {
    const routesToTRF = filterByArrivalIata(data, 'TRF');
    console.log(routesToTRF);
});
```

3.	Displaying routes in a more readable format

```
function displayRoute(route) {
    console.log(`Flight No: ${route.flightNumber}`);
    console.log(`From ${route.departureIata} (${route.departureTime}) to ${route.arrivalIata} (${route.arrivalTime})`);
    console.log(`Airlines: ${route.airlineIata}`);
    console.log(`Registration Numbers: ${route.regNumber}`);
}

// Example usage
fetchData().then(data => {
    data.forEach(route => {
        displayRoute(route);
        console.log('----------------------');  // Separator
    });
});
```

### Response
```
[
{ "departureIata": "IAH",
"departureIcao": "KIAH",
"departureTerminal": A29,
"departureTime": "08:00:00",
"arrivalIata": "DFW",
"arrivalIcao": "KDFW",
"arrivalTerminal": A37,
"arrivalTime": "09:23:00",
"airlineIata": "AA",
"airlineIcao": "AAL",
"flightNumber": "2668",
"codeshares": null,
"regNumber": "N581UW"
}
]
```

### Access & Support
[Get your API key](https://aviation-edge.com/premium-api/) in a minute and start testing the data right away. The API is provided through API subscriptions. All plans grant access to the Future Schedules API and other APIs with a difference of the monthly API call limit. Choose the best plan for you and upgrade, downgrade or cancel your plan anytime without  commitments.

[Contact us](https://aviation-edge.com/contact/) via email for any questions or support requests.

### License
The use of the API is subject to Aviation Edge [Terms and Conditions](https://aviation-edge.com/api-terms-of-service/).
