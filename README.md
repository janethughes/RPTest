#Relying Party On-boarding

##RP On-boarding Guide v0.6

###Introduction

The [Identity Assurance Hub Service SAML v2.0 Profile](https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/263459/Identity_Assurance_Hub_Service_Profile_v1.1a.pdf) describes how service providers offering online government services can use any number of Hub Services for the brokering of a citizen authentication and enrichment of citizen attributes. 

This document describes how (????)[Service Providers], Identity Providers and Matching Service Providers can gain access to the hub service as part of an on-boarding process with the IDA service. 

#[Tokening Service](https://github.com/ianimeson/RPTest/blob/master/Tokening%20Service)

##Overview
The token service is a mechanism designed to reduce the possibility of fraud in the live IDAP hub during the private beta phase. It will work by providing tokens, that can then be used to ensure only invited users are able to access the hub service.
The token service implemented by the IDA team will provide a restful (http/json) interface to generate new tokens, check whether a token is valid and revoke tokens.
The relying party will typically request a number of tokens from the token service, store them and distribute them to users (for example, in an email inviting them to try the IDAP service).
Before a user is sent to the live IDAP hub with an AuthN request, the Relying party must get the token from the user and validate it against the token service. If the token is invalid, the user must not be redirected to the hub, if the token is valid, an AuthN request can be sent to the hub as usual.
The token service will limit the number of times a single token can be used per day, and in total, and will expire old tokens. The Relying party will also have the option to revoke a token if it is no longer needed, or they have detected fraud for example.
Process Flow
A typical token authentication will use the process below:
How the users provide the token and when in the flow of the transaction is up to the relying party. The relying party must ensure that a user has provided a token, which has been checked to be valid, before sending a SAML authn request to the hub. It is also important that the user cannot bypass the token validation at the relying party. The relying party must check the validity of the token on every single authentication by the user, the result must not be cached.
The IDA team will provide a test version of the token service for relying parties to develop their token validating code. This test token service can also be used to generate and validate tokens in automated or manual testing with the test hub. Relying parties will be able to generate as many tokens as they wish on the test token service.
Validation Rules
A token is valid if it satisfies the following:
• The token exists.
• The token has not been revoked (either by the relying party or the IDAP team)
• The token has not expired (currently configured to 90 calendar days from the time of creation)
• The token has not been used successfully 5 times already on the current calendar day.
• The token has not been used successfully 15 times already in the last 90 calendar days.
All of the above thresholds are configurable and may be subject to change. The same rules and thresholds will apply to the test token service.
Token generation
Tokens consist of a string of alpha numeric characters. The length is configurable and may change, but is currently set to 12 characters.
The live token service will allow a relying party to generate a limited number of tokens only. If more tokens are required, the relying party must contact the IDAP team, and more tokens will be allocated.
The test service will allow a relying party to generate an unlimited number of tokens, so automated and manual testing processes can request new tokens every time.
Authentication
The token service (and test token service) will require authentication via basic auth. Credentials will be supplied by the IDAP team.





