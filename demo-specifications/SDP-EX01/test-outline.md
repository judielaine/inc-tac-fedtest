# SDP-EX01

_Put [deployment statement](https://kantarainitiative.github.io/SAMLprofiles/saml2int.html) here; see [Deployment statements to test csv file](../../test-specifications/deployment-statements-to-test.csv)_ Example deployments must display the same organization name on the website's "Example" page to authenticated users as is in the metadata. (Note artificial example where we pretend both IdPs and SPs have a page that shows the respective SPs or IdPs involved in the authN flow. Purely artificial example.)

## Summary of test

Using the test subject and a known locale, authenticate, and then visit the test subject's Example page. 

_Indicate the test parties in the table and how differences in configuration affect the test configuration_

|  | subject | multilateral  | bilateral |
|---:|:---:|---|---|
| IdP | X | X | X |
| SP | X | X | X |
| registrant | _1_ |  |  |

### Comments

This test can be combined with other successful authentication flow tests; the "Example" page could be verified each time.

1. The registration should be checked for the presence of the mdui:DisplayName

### Additional references

_List any resources that may be helpful in designing and understanding the test._

1. https://wiki.refeds.org/display/FBP/MDUI+-+Federation+recommendations
2. https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-metadata-ui/v1.0/os/sstc-saml-metadata-ui-v1.0-os.html#__RefHeading__10385_1021935550
3. https://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf for type="md:localizedNameType"/

## IdP test

### Configuration requested from test subject

_What information is needed from or about the test subject to configure testing scripts?_ 

1. MUST have credentials for a valid test user (no special requirements to claims)
2. MUST have URL for IdP's "Example" page

If no "Example" page can be given, all SDP-EX01 tests score a Fail in calculating the results, no testing occurs.

### Testing SP metadata configuration

_What metadata is needed test targets for the test?_ 

1. MUST have a SP01 with mdui:DisplayName entries, all over 30 characters long, for multiple locales including a common locale and one very esoteric locale. All names should have a distinct and  uncommon combination of characters for the first ten characters.
2. COULD have a SP02 with single mdui:DisplayName, over 30 characters long, for a locale also present in the first SP, where the first ten characters are distinct from other mdui:DisplayName and uncommon. This is the minimum complication test configuration. 
3. COULD have a SP03 with a malformed mdui:DisplayName missing a locale, where the first ten characters are distinct from other mdui:DisplayName and uncommon. Keeping this separate from SP01 ensures that the missing locale is not confused with an error reading multiple entries.
4. COULD have a SP04 SP with a 256 character length mdui:DisplayName in the common locale. See [SDP-G02](https://kantarainitiative.github.io/SAMLprofiles/saml2int.html#_general)  Keeping this separate from SP01 ensures that the long value is not confused with an error reading multiple entries.

### Testing SP configuration

_What features are needed for the SP?_ 

The minimum needed from the SP is a url that triggers the authentication flow to the IdP being tested and an ACS to complete the authentication.

### Test client configuration

1. MUST be able to change the locale of the test client for each authentication loop.

### Test steps 

_List testing steps, including the scorable tests labeled as SDP-XXXX.## . Include  scoring outcomes of either "PASS, FAIL" or "BEST, BETTER, GOOD, ACCEPTABLE, BUGGY, FAIL"

1. Loop 1 for "common locale". 
   1. Configure the test client with the common locale available in SP01, SP02, and SP04.
   2. Start authentication at SP01.
   3. Visit IdP Example page. 
      1. String search for the SP01's locale-appropriate name. (If found SDP-EX01.01 = BEST: the full mdui:DisplayName is displayed.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.01 = GOOD: some truncation has occurred.) 
      3. If not found, string search for the leading ten characters of all mdui:DisplayName entries. (IF found SDP-EX01.01 = ACCEPTABLE: display of mdui:DisplayName, but not locale appropriate. Could record the mdui:DisplayName presented for later diagnostics.)
         1. If not found, AND SP02 present, note that possible failure cause is encountering more than one mdui:DisplayName. Determine if not displaying at all or if buggy:
            1. Use SSO for second SP
            2. Visit IdP Example page.
               1. String search for the second SP's locale-appropriate name. (If found SDP-EX01.01 = BUGGY: the full mdui:DisplayName is displayed.)
               2. If not found, string search for the leading ten characters. (If found SDP-EX01.01 = BUGGY: some truncation has occurred.) 
               3. If not found, SDP-EX01.01 = FAIL, abort test and score all tests as fail.
         2. If not found, and SP02 not available, SDP-EX01.01 = FAIL
   4. Use SS0 for SP03 (if available)
   5. Visit IdP Example page. 
      1. String search for the SP03's name. (If found SDP-EX01.02 = PASS: the full mdui:DisplayName is displayed despite bad data.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.02 = PASS: some truncation has occurred but displays despite bad data.)
      3. If not found,  SDP-EX01.02 = FAIL, the implementation cannot deal with bad data.
   6. Use SS0 for SP04 (if available)
   5. Visit IdP Example page. 
      1. String search for the SP04's name. (If found SDP-EX01.03 = PASS: the full mdui:DisplayName is displayed despite bad data.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.03 = PASS: some truncation has occurred but displays despite bad data.)
      3. If not found,  SDP-EX01.03 = FAIL, the implementation cannot deal with long value.
2. Loop 2 for "uncommon locale" present in the first SP. 
   1. Configure the test client with the common locale available in SP01
   2. Start authentication at SP01.
   3. Visit IdP Example page. 
      1. String search for the first SP's locale-appropriate name. (If found SDP-EX01.04 = BEST: the full mdui:DisplayName is displayed.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.04 = GOOD: some truncation has occurred.) 
      3. If not found, string search for the leading ten characters of all mdui:DisplayName entries. (IF found SDP-EX01.04 = ACCEPTABLE: display of mdui:DisplayName, but not locale appropriate.)
      4. If not found, SDP-EX01.04 = FAIL: despite the presence of a value, the display fails for uncommon locale.
3. Loop 3 for "uncommon locale" NOT in the first SP. 
   1. Configure the test client with an uncommon locale NOT available in SP01
   2. Start authentication at SP01.
   3. Visit IdP Example page. 
      1. String search for the leading ten characters of all mdui:DisplayName entries. (IF found SDP-EX01.05 = PASS: display of mdui:DisplayName, but not locale appropriate.)
      2. If not found, SDP-EX01.05 = FAIL: display fails for when client's locale not present in available locales.

### Tests and weighting

_For each test, identify the scoring and whether it is available to bilateral and/or multilateral configurations. Provide a weighting for the importance of each test._ 

| test number | description | pass/fail | best/../fail | weight |
|---:|---|:---:|---|---|
| 01 | Test to see if locale appropriate name is selected from multiple locales. |  | M, B | 50 |
| 02 | Test to verify support for mdui:DisplayName missing a locale. | M |  | 5 |
| 03 | Test to verify support for a 256 character mdui:DisplayName. | M |  | 5 |
| 04 | Test to verify appropriate selection of locale regardless of locales available on the site. |  | M,B | 20 |
| 05 | Test to determine reasonable fail over when client locale is not available. |    M,B |  | 20 |

## SP test

### Configuration requested from test subject

_What information is needed from or about the test subject to configure testing scripts?_ 

1. MUST have URL for IdP's "Example" page
2. MUST have the URL to start authentication to IDP01
3. COULD have the URL to start authentication to IDP02, IDP03, and IDP04.
4. COULD have URL to clear authentication state

If no "Example" page can be given, all SDP-EX01 tests score a Fail in calculating the results, no testing occurs.

### Testing IDP metadata configuration

_What metadata is needed test targets for the test?_ 

1. MUST have a IDP01 with mdui:DisplayName entries, all over 30 characters long, for multiple locales including a common locale and one very esoteric locale. All names should have a distinct and  uncommon combination of characters for the first ten characters.
2. COULD have a IDP02 with single mdui:DisplayName, over 30 characters long, for a locale also present in the first SP, where the first ten characters are distinct from other mdui:DisplayName and uncommon. This is the minimum complication test configuration. 
3. COULD have a IDP03 with a malformed mdui:DisplayName missing a locale, where the first ten characters are distinct from other mdui:DisplayName and uncommon. Keeping this separate from SP01 ensures that the missing locale is not confused with an error reading multiple entries.
4. COULD have a IDP04 SP with a 256 character length mdui:DisplayName in the common locale. See [SDP-G02](https://kantarainitiative.github.io/SAMLprofiles/saml2int.html#_general)  Keeping this separate from SP01 ensures that the long value is not confused with an error reading multiple entries.

### Testing SP configuration

_What features are needed for the IDP?_ 

The minimum needed from the IdP is a valid principal.

### Test client configuration

1. MUST be able to change the locale of the test client for each loop.

### Test steps 

_List testing steps, including the scorable tests labeled as SDP-XXXX.## . Include  scoring outcomes of either "PASS, FAIL" or "BEST, BETTER, GOOD, ACCEPTABLE, BUGGY, FAIL"

1. Loop 1 for "common locale". 
   1. Configure the test client with the common locale available in IDP01, IDP02, and IDP04.
   2. Start authentication at the test SP for IDP01
   3. Visit SP Example page. 
      1. String search for IDP01's locale-appropriate name. (If found SDP-EX01.01 = BEST: the full mdui:DisplayName is displayed.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.01 = GOOD: some truncation has occurred.) 
      3. If not found, string search for the leading ten characters of all mdui:DisplayName entries. (IF found SDP-EX01.01 = ACCEPTABLE: display of mdui:DisplayName, but not locale appropriate. Could record the mdui:DisplayName presented for later diagnostics.)
         1. If not found, AND IDP02 present, note that possible failure cause is encountering more than one mdui:DisplayName. Determine if not displaying at all or if buggy:
            1. Clear authentication state
            2. Start authentication at the test SP for IDP02
            3. Visit SP Example page.
               1. String search for the second IDP's locale-appropriate name. (If found SDP-EX01.01 = BUGGY: the full mdui:DisplayName is displayed.)
               2. If not found, string search for the leading ten characters. (If found SDP-EX01.01 = BUGGY: some truncation has occurred.) 
               3. If not found, SDP-EX01.01 = FAIL, abort test and score all tests as fail.
         2. If not found, and IDP02 not available, SDP-EX01.01 = FAIL
   4. Clear authentication state
   5. Start authentication at the test SP for IDPO3 (if available)
   6. Visit SP Example page. 
      1. String search for the IDP03's name. (If found SDP-EX01.02 = PASS: the full mdui:DisplayName is displayed despite bad data.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.02 = PASS: some truncation has occurred but displays despite bad data.)
      3. If not found,  SDP-EX01.02 = FAIL, the implementation cannot deal with bad data.
   7. Clear authentication state
   8. Start authentication at the test SP for IDP04 (if available)
   9. Visit IdP Example page. 
      1. String search for the IDP04's name. (If found SDP-EX01.03 = PASS: the full mdui:DisplayName is displayed despite bad data.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.03 = PASS: some truncation has occurred but displays despite bad data.)
      3. If not found,  SDP-EX01.03 = FAIL, the implementation cannot deal with long value.
2. Loop 2 for "uncommon locale" present in the first SP. 
   1. Configure the test client with the common locale available in IDP01
   2. Start authentication at the test SP for IDP01
   3. Visit SP Example page. 
      1. String search for IDP01's locale-appropriate name. (If found SDP-EX01.04 = BEST: the full mdui:DisplayName is displayed.)
      2. If not found, string search for the leading ten characters. (If found SDP-EX01.04 = GOOD: some truncation has occurred.) 
      3. If not found, string search for the leading ten characters of all mdui:DisplayName entries. (IF found SDP-EX01.04 = ACCEPTABLE: display of mdui:DisplayName, but not locale appropriate.)
      4. If not found, SDP-EX01.04 = FAIL: despite the presence of a value, the display fails for uncommon locale.
3. Loop 3 for "uncommon locale" NOT in IDP01. 
   1. Configure the test client with an uncommon locale NOT available in IDP01
   2. Start authentication at the test SP for IDP01
   3. Visit SP Example page. 
   4. Visit IdP Example page. 
      1. String search for the leading ten characters of all mdui:DisplayName entries. (IF found SDP-EX01.05 = PASS: display of mdui:DisplayName, but not locale appropriate.)
      2. If not found, SDP-EX01.05 = FAIL: display fails for when client's locale not present in available locales.

### Tests and weighting

_For each test, identify the scoring and whether it is available to bilateral and/or multilateral configurations. Provide a weighting for the importance of each test._ 

| test number | description | pass/fail | best/../fail | weight |
|---:|---|:---:|---|---|
| 01 | Test to see if locale appropriate name is selected from multiple locales. |  | M, B | 50 |
| 02 | Test to verify support for mdui:DisplayName missing a locale. | M |  | 5 |
| 03 | Test to verify support for a 256 character mdui:DisplayName. | M |  | 5 |
| 04 | Test to verify appropriate selection of locale regardless of locales available on the site. |  | M,B | 20 |
| 05 | Test to determine reasonable fail over when client locale is not available. |    M,B |  | 20 |
