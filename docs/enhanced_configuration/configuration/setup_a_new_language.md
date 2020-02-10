#  Setup a new language 

If a new siteaccess is introduced it is important to add a new policy for the anonymous user role:

**Allow login for the new siteaccess\!**

![](attachments/23560343/23563561.png)

# Configurations

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Path
</th>
<th><div class="tablesorter-header-inner">
File
</th>
<th><div class="tablesorter-header-inner">
Configuration
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td>app/config</td>
<td>ezplatform.yml</td>
<td><p>ezpublish.siteaccess.list</p>
<p>ezpublish.siteaccess.groups.ezdemo_site_clean_group</p>
<p><br />
</p>
<p>ezpublish.siteaccess.match.Compound\LogicalAnd</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p>ezpublish.siteaccess.ses_admin.languages:</p>
<p>ezsettings.default.languages:</p></td>
<td><p>General configuration for new siteaccess</p>
<p>Set new siteaccess to general group "ezdemo_site_clean_group"</p>
<p><br />
</p>
<p>Define the domain and language for this new siteaccess, e.g.</p>
<div>

<p><br />
</p>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><div class="container" title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode php">website_be_nl:</code>

<div class="line number2 index1 alt1">
<code class="sourceCode php">    </code><code class="sourceCode php">matchers:</code>

<div class="line number3 index2 alt2">
<code class="sourceCode php">        </code><code class="sourceCode php">Map\<span class="kw">URI:</code>

<div class="line number4 index3 alt1">
<code class="sourceCode php">            </code><code class="sourceCode php">nl: <span class="kw">true</code>

<div class="line number5 index4 alt2">
<code class="sourceCode php">        </code><code class="sourceCode php">Map\Host:</code>

<div class="line number6 index5 alt1">
<code class="sourceCode php">            </code><code class="sourceCode php">%domain_website_be%: <span class="kw">true</code>

<div class="line number7 index6 alt2">
<code class="sourceCode php">    </code><code class="sourceCode php"><span class="kw">match: website_be_nl</code>

<div class="line number8 index7 alt1">
</td>
</tr>
</tbody>
</table>

<p><br />
</p>
<p><br />
</p>
<p>Configuration for session name and used languages.</p>
<div>

<p><br />
</p>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><div class="container" title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode php">website_be_nl:            </code>

<div class="line number2 index1 alt1">
<code class="sourceCode php">    </code><code class="sourceCode php">session:</code>

<div class="line number3 index2 alt2">
<code class="sourceCode php">        </code><code class="sourceCode php">name: eZSESSID</code>

<div class="line number4 index3 alt1">
<code class="sourceCode php">    </code><code class="sourceCode php">languages:</code>

<div class="line number5 index4 alt2">
<code class="sourceCode php">        </code><code class="sourceCode php">- dut-<span class="kw">NL</code>

</td>
</tr>
</tbody>
</table>

<p><br />
</p>
<p><br />
New language must be enabled for admin siteaccess.</p>
<p>New language must be added.</p></td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [image2016-9-9 14:49:33.png](attachments/23560343/23563561.png) (image/png)  
