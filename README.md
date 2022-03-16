# Vehicle_Classification_with_NumberPlate_Detection

<p>
    <img src="https://img.shields.io/npm/l/color-calendar?style=flat-square" alt="license" />
</p>

<p>
    A vehicle detection, classifcation and counting model used to maintain a record of all the vehicles passing an area at a particular time. This comes with a license plate detection which can be used to keep a record of all those who break some traffic rule.

</p>


- [Features](#features)
- [Installation](#Installation)
- [Usage](#usage)
- [Methods](#methods)
- [Bug Reporting](#bug)
- [Feature Request](#feature-request)
- [Release Notes](#release-notes)
- [License](#license)

<a id="features"></a>

## 🚀 Features

- Zero dependencies
- Detection of vehicles in a dynamic setting i.e. Video
- Detection of Vehicles in a static setting i.e. a particular frame
- Highlighting the vehicle along with the prediction accuracy.
- Counter for different types of vehicles i.e. Cars, Trucks, Bikes.
- Number plate Detection System for those who break any rules. (Upcoming)
- Alert for all the vehicles who break any rules. (Upcoming)
- Storing the data about the number of vehicles passed segregated by their category. 
- [Request more features](#feature-request)...

<a id="Installation"></a>
  ## Installation
### Directly 
```bash
pip install -r requirements.txt
```
#### Or
### Use a Virtual Environment

```bash
    virtualenv <virtual_environment_name>
    
    cd <virtual_environment_name>\Scripts
    
    activate.bat
    
    pip install -r requirements.txt
```



<a id="usage"></a>
## 🔨 Usage




<a id="methods"></a>

## 🔧 Methods

### `reset()`

Reset calendar to today's date.

```javascript
// Example
let myCal = new Calendar();
myCal.reset();
```

### `setDate()`

Props:
| Props | Type | Required | Description |
|-------|------|----------|--------------------|
| date | Date | required | New date to be set |

Set new selected date.

```javascript
// Example
myCal.setDate(new Date());
```

### `getSelectedDate()`

Return:

- Type: `Date`
- Description: `Currently selected date`

Get currently selected date.

### `getEventsData()`

Return:

- Type: [EventData](#type-event-data)[]
- Description: `All events`

Get events array.

### `setEventsData()`

Props:
| Props | Type | Required | Description |
|--------|------------|----------|------------------|
| events | [EventData](#type-event-data)[] | required | Events to be set |

Return:

- Type: `Number`
- Description: `Number of events set`

Set a new events array.

### `addEventsData()`

Props:
| Props | Type | Required | Description |
|--------|------------|----------|--------------------|
| events | [EventData](#type-event-data)[] | required | Events to be added |

Return:

- Type: `Number`
- Description: `Number of events added`

Add events of the events array.

### `setWeekdayDisplayType()`

Props:
| Props | Type | Required | Description |
|--------|------------|----------|--------------------|
| weekdayDisplayType | [WeekdayDisplayType](#type-weekday-display-type) | required | New weekday type |

Set weekdays display type.

```javascript
// Example
myCal.setWeekdayDisplayType("short");
```

### `setMonthDisplayType()`

Props:
| Props | Type | Required | Description |
|--------|------------|----------|--------------------|
| monthDisplayType | [MonthDisplayType](#type-month-display-type) | required | New month display type |

Set month display type.



<a id="bug"></a>

## 🐛 Bug Reporting

Feel free to [open an issue](https://github.com/deepanshug4/Vehicle_Classification_with_NumberPlate_Detection/issues) on GitHub if you find any bug.

<a id="feature-request"></a>

## ⭐ Feature Request

- Feel free to [Open an issue](https://github.com/deepanshug4/Vehicle_Classification_with_NumberPlate_Detection/issues) on GitHub to request any additional features you might need for your use case.
- Connect with me on [LinkedIn](https://www.linkedin.com/in/deepanshug4/). I'd love ❤️️ to hear where you are using this library.

<a id="release-notes"></a>

## 📋 Release Notes

Check [here](https://github.com/deepanshug4/Vehicle_Classification_with_NumberPlate_Detection/releases) for release notes.

<a id="license"></a>

## 📜 License

This software is open source, licensed under the [MIT License](https://github.com/PawanKolhe/color-calendar/blob/master/LICENSE).
