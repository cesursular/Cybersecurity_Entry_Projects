# VPN Log Analysis and Reporting

This project provides a set of Splunk queries to analyze VPN logs for security and usage reporting purposes. The repository contains various Splunk queries to identify the most connected IPs, CPU usage per user, countries with the most connection attempts, and users who experienced the most connection failures. 

## Project Structure

- `splunk_queries/`: Contains Splunk Search Processing Language (SPL) queries.
  - `most_connected_ip.spl`: Query to find the most connected IP.
  - `user_cpu_usage.spl`: Query to calculate CPU usage per user.
  - `countries_connection_attempts.spl`: Query to list countries with the most connection attempts.
  - `failed_connections.spl`: Query to find users with the most connection failures.
- `sample_logs/`: Contains sample VPN log files.
  - `VPN-logs-1663593355154.json`: Sample VPN log file.
- `dashboards/`: Contains JSON configuration for a Splunk dashboard.
  - `vpn_analysis_dashboard.json`: A dashboard configuration to visualize the queries.

## Prerequisites

1. **Install Splunk**: Install Splunk on your local machine or server. Follow the instructions on the [official Splunk website](https://www.splunk.com) to download and set up Splunk.
2. **Upload Sample Logs**: Use the sample logs provided in the `sample_logs/` folder to test the queries.

## Queries

This section provides detailed descriptions of the Splunk queries available in this repository. Each query is designed to address a specific aspect of VPN log analysis.

### 1. Most Connected IP
Identifies the most frequently connected IP address in the VPN logs.

- **File**: `splunk_queries/most_connected_ip.spl`
- **Query**:
  ```spl
  index=main | stats count by Source_ip | sort - count
      Description: This query calculates the number of connections from each source IP and sorts them in descending order to find the most connected IP.
    Result: The most connected IP is 172.201.60.191.

2. CPU Usage per User

Calculates the VPN connection duration for each user, which can be used to estimate CPU usage or network usage for each user session.

    File: splunk_queries/user_cpu_usage.spl
    Query:

    spl

    index=vpn_logs action="built" OR action="teardown"
    | transaction UserName startswith="action=built" endswith="action=teardown"
    | eval duration=tostring(duration, "duration")
    | table UserName, Source_ip, duration

    Description: This query uses the transaction command to calculate the duration of each user’s VPN connection session based on "built" and "teardown" actions.

3. Countries with Most Connection Attempts

Lists the countries with the most VPN connection attempts, which can help identify possible geo-based threats or interests.

    File: splunk_queries/countries_connection_attempts.spl
    Query:

    spl

    index=main | stats count by Source_Country | sort - count

    Description: This query groups the connection attempts by the source country and sorts them in descending order to identify which countries have the most attempts.

4. Users with Most Connection Failures

Identifies users who experienced the most connection failures, which could indicate connectivity issues or potential security concerns.

    File: splunk_queries/failed_connections.spl
    Query:

    spl

    index=main action="teardown"
    | stats count by UserName, Source_ip, action
    | where count > 1

    Description: This query identifies users with multiple "teardown" actions, indicating repeated failed attempts to connect.

Usage

    Run Queries: Copy and paste the queries from the splunk_queries/ folder into Splunk's search bar.
    Create Dashboards: Use the provided JSON configuration in the dashboards/ folder to create a Splunk dashboard for a more visual representation of the data.

Dashboards

The dashboards/ folder contains a sample JSON file (vpn_analysis_dashboard.json) that can be imported into Splunk to visualize the queries in a structured dashboard.
Sample Logs

The sample_logs/ folder contains a sample VPN log file (VPN-logs-1663593355154.json) that can be uploaded to Splunk to test and visualize the queries.

License

This project is licensed under the MIT License - see the LICENSE file for details.
  
