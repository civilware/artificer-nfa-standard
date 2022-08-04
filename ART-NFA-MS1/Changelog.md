### Artificer NFA Standard Changelog

###1.1.0

* Listings now have an explicit cap end duration of 7 days
* In the event of an auction listing, upon being outbid the previous bidder is refunded within the same call.
* Initial STORE()s to be modified by creators has been retained in InitializePrivate() while the remainder moved to an init() function for easier reading/consumption as well as code / standard checking in future.
* Functions markdown file created giving some more detail to each public function that is able to be called and their respective parameters.
* Version STORE() appended and will retain moving forward

###1.0.0

* ART-NFA-MS1 Implemented