## Historical

Before October 2023, the Beacon Final test program was writing an audit log into a SQL Server database residing on a server called `PROD-VM` which was located at ACR and accessed over a VPN connection. The database was named "OSL_V5" and contained an audit log of passed final tests at both ACR and OSL.

After October 2024 a decision was taken to take down the VPN link. Since the database configuration (including connection details and login credentials) was all hard-coded in the Final Test program, this program could not operate without the database on PROD-VM and therefore a replica was created in the UK (by BCS) and everything was named to be identical to the ACR copy to allow Final Test to continue to operate. The recovered data was restored and production continued.

The OSL-V5 table therefore contains:
 - OSL+ACR final test results up to October 2023
 - OSL test results after October 2023

The ACR copy similarly contains only ACR data after October 2023, and the two databases are no longer connected in any way.

### Beacon Final Test

Beacon Final Test writes passing test results to the `OSL_V5` table in the `TEST_DATA` database on the `PROD-VM` SQL Server.

The information written serves as an audit log and is used to display the 6 most recently tested beacons on each test station.

### Final Test Next Generation

In November 2025 a second database was added for the new "Next Generation" final test that is used for MOB2, M200 and S200. The new database is stored in the `FinalTestNextGeneration-Production` database and has several tables. It contains a lot more detailed information including all the measured values taken during each test session and is very useful for forensic investigations. For example, we were able to analyse all the AIS RF Feedback Voltage values over several months of production and determine that the measurements were rather "noisy". We used this data to drive improvements to Final Test so that it obtained more consistent values.
## Physical

The PROD-VM server is located in a rack in the server room off the canteen
## Data Safeguarding

The database server on PROD-VM is part of the regular backup schedule that happens several times a day and is administered by MJ, the UK IT manager and thus it is protected to the same degree as the serial numbers files and other data on the R drive.
