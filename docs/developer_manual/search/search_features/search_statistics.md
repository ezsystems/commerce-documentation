#  Search Statistics 

eZ Commerce provides a functionality to display the searches that are performed by the users. Every search query is logged into the database table: 

    ses_log_search

``` 
mysql> select * from ses_log_search order by log_timestamp DESC limit 20;
+------+---------------------+---------------------+-----------+-------------+-------------------+----------------------------+---------+---------+----------+---------+
| id   | log_timestamp       | log_channel         | log_level | log_message | request_id        | session_id                 | user_id | results | language | shop_id |
+------+---------------------+---------------------+-----------+-------------+-------------------+----------------------------+---------+---------+----------+---------+
| 3823 | 2017-02-17 12:35:28 | silver_eshop_search |       200 | a           | el8691ce-000c4cc7 | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3822 | 2017-02-17 12:34:56 | silver_eshop_search |       200 | b           | el8691ce-fe01c18d | el8691celljual0kndes20tsl7 | 10      |       2 | ger-DE   | MAIN    |
| 3821 | 2017-02-17 12:34:22 | silver_eshop_search |       200 | a           | el8691ce-fbe39c67 | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3820 | 2017-02-17 12:34:00 | silver_eshop_search |       200 | a           | el8691ce-fa89ecde | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3819 | 2017-02-17 12:33:23 | silver_eshop_search |       200 | x2          | el8691ce-f838dbbb | el8691celljual0kndes20tsl7 | 10      |       0 | ger-DE   | MAIN    |
| 3818 | 2017-02-17 12:32:36 | silver_eshop_search |       200 | x           | el8691ce-f5473c35 | el8691celljual0kndes20tsl7 | 10      |       2 | ger-DE   | MAIN    |
| 3817 | 2017-02-17 12:29:27 | silver_eshop_search |       200 | a           | el8691ce-e97d9b83 | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3816 | 2017-02-17 12:27:46 | silver_eshop_search |       200 | a           | el8691ce-e3259dc3 | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3815 | 2017-02-17 12:26:03 | silver_eshop_search |       200 | a           | el8691ce-dcb4b677 | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3814 | 2017-02-17 12:24:40 | silver_eshop_search |       200 | a           | el8691ce-d78a57db | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3813 | 2017-02-17 12:24:16 | silver_eshop_search |       200 | a           | el8691ce-d6021990 | el8691celljual0kndes20tsl7 | 10      |      90 | ger-DE   | MAIN    |
| 3812 | 2017-02-17 12:24:02 | silver_eshop_search |       200 | e           | el8691ce-d52da26b | el8691celljual0kndes20tsl7 | 10      |      16 | ger-DE   | MAIN    |
| 3811 | 2017-02-17 12:23:56 | silver_eshop_search |       200 | e           | el8691ce-d4cc4fa1 | el8691celljual0kndes20tsl7 | 10      |      16 | ger-DE   | MAIN    |
| 3810 | 2017-02-17 12:20:02 | silver_eshop_search |       200 | e           | el8691ce-c62034df | el8691celljual0kndes20tsl7 | 10      |      16 | ger-DE   | MAIN    |
| 3809 | 2017-02-17 12:19:10 | silver_eshop_search |       200 | e           | el8691ce-c2ed2067 | el8691celljual0kndes20tsl7 | 10      |      16 | ger-DE   | MAIN    |
| 3808 | 2017-02-17 12:17:27 | silver_eshop_search |       200 | b           | 5vqrni12-bc78d083 | 5vqrni12v36b3mrsj2shcgilj4 | 10      |       2 | ger-DE   | MAIN    |
| 3807 | 2017-02-17 12:16:30 | silver_eshop_search |       200 | a           | 5vqrni12-b8eac0b5 | 5vqrni12v36b3mrsj2shcgilj4 | 10      |      90 | ger-DE   | MAIN    |
| 3806 | 2017-02-17 12:16:07 | silver_eshop_search |       200 | d           | 5vqrni12-b77df822 | 5vqrni12v36b3mrsj2shcgilj4 | 10      |       1 | ger-DE   | MAIN    |
| 3805 | 2017-02-17 12:15:54 | silver_eshop_search |       200 | c           | 5vqrni12-b6a1caed | 5vqrni12v36b3mrsj2shcgilj4 | 10      |       0 | ger-DE   | MAIN    |
| 3804 | 2017-02-17 12:14:16 | silver_eshop_search |       200 | b           | 5vqrni12-b0879554 | 5vqrni12v36b3mrsj2shcgilj4 | 10      |       2 | ger-DE   | MAIN    |
+------+---------------------+---------------------+-----------+-------------+-------------------+----------------------------+---------+---------+----------+---------+
20 rows in set (0.01 sec)
```

This search statistics can be displayed in eCommerce cockpit

This logic is generated in the following service:

    QuerySearchLogService

There are 3 tables displayed

<table>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<h3 id="SearchStatistics-Topsearchterms">Top search terms</h3>
<p>Description:</p>
<pre><code>Get the most searched terms order by times searched.</code></pre>
<p>Query:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$queryBuilder = $this-&gt;searchOrmLogRepository-&gt;createQueryBuilder(&#39;ses_log_search&#39;)
    -&gt;select(
        array(
            &#39;ses_log_search.logMessage&#39;,
            &#39;count(ses_log_search.logMessage) as amount&#39;,
            &#39;avg(ses_log_search.results) as hits&#39;
        ))
    -&gt;where(&#39;ses_log_search.logLevel = 200&#39;)
    -&gt;andWhere(&#39;ses_log_search.shopId = :shopId &#39;)
    -&gt;groupBy(&#39;ses_log_search.logMessage&#39;)
    -&gt;orderBy(&#39;amount&#39;, &#39;DESC&#39;)
    -&gt;setMaxResults($limit)
    -&gt;setParameter(&#39;shopId&#39;, $shopId);</code></pre>
</td>
<td><div class="content-wrapper">
<img src="attachments/23560968/23563493.png" class="confluence-embedded-image" height="400" />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<h3 id="SearchStatistics-Lastsearchterms">Last search terms</h3>
<p>Description:</p>
<pre><code>Get the last searches order by date and time.</code></pre>
<p>Query:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$queryBuilder = $this-&gt;searchOrmLogRepository-&gt;createQueryBuilder(&#39;ses_log_search&#39;)
    -&gt;select(
        array(
            &#39;ses_log_search.logTimestamp&#39;,
            &#39;ses_log_search.logMessage&#39;,
            &#39;ses_log_search.results&#39;
        ))
    -&gt;where(&#39;ses_log_search.logLevel = 200&#39;)
    -&gt;andWhere(&#39;ses_log_search.shopId = :shopId &#39;)
    -&gt;orderBy(&#39;ses_log_search.logTimestamp&#39;, &#39;DESC&#39;)
    -&gt;setMaxResults($limit)
    -&gt;setParameter(&#39;shopId&#39;, $shopId);
</code></pre>
</td>
<td><div class="content-wrapper">
<img src="attachments/23560968/23563491.png" class="confluence-embedded-image" height="400" />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
Most searched terms with less results
<pre><code>Get the most searched terms with less hits.</code></pre>
<pre><code>This means something that the user is interested in, but with no results.</code></pre>
<p>Query:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$queryBuilder = $this-&gt;searchOrmLogRepository-&gt;createQueryBuilder(&#39;ses_log_search&#39;)
    -&gt;select(
        array(
            &#39;ses_log_search.logMessage&#39;,
            &#39;count(ses_log_search.logMessage) as amount&#39;,
            &#39;sum(ses_log_search.results) as hits&#39;
        ))
    -&gt;where(&#39;ses_log_search.logLevel = 200&#39;)
    -&gt;andWhere(&#39;ses_log_search.shopId = :shopId &#39;)
    -&gt;groupBy(&#39;ses_log_search.logMessage&#39;)
    -&gt;orderBy(&#39;hits&#39;, &#39;ASC&#39;)
    -&gt;addOrderBy(&#39;amount&#39;, &#39;DESC&#39;)
    -&gt;setMaxResults($limit)
    -&gt;setParameter(&#39;shopId&#39;, $shopId);</code></pre>
</td>
<td><div class="content-wrapper">
<img src="attachments/23560968/23563503.png" class="confluence-embedded-image" height="400" />
</td>
</tr>
</tbody>
</table>

### Additional notes

Search queries from main search box are logged

Search queries from second search box are logged only if:

  - No facets are selected
  - Search term is different than previous search term.

## Attachments:

![](images/icons/bullet_blue.gif) [Screen Shot 2017-02-23 at 16.34.48.png](attachments/23560968/23563493.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2017-02-23 at 16.35.02.png](attachments/23560968/23563491.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2017-02-23 at 16.35.13.png](attachments/23560968/23563503.png) (image/png)  
