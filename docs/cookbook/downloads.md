#  Downloads 

Logic how to handle downloads is not implemented in silver-e.shop yet.

However there is a prepared entity to store the downloads data in the shop.

<table>
<tbody>
<tr>
<td><p><strong>Field</strong></p></td>
<td><p><strong>Type in Shop</strong></p></td>
<td><p><strong>Type in DB</strong></p></td>
<td><p><strong>Required</strong></p></td>
<td><p><strong>Description</strong></p></td>
</tr>
<tr>
<td><pre><code>id</code></pre></td>
<td><pre><code>int</code></pre></td>
<td><pre><code>int</code></pre></td>
<td><pre><code>true</code></pre></td>
<td><pre><code>Unique ID</code></pre></td>
</tr>
<tr>
<td><pre><code>basketId</code></pre></td>
<td><pre><code>int</code></pre></td>
<td><pre><code>int</code></pre></td>
<td><pre><code>true</code></pre></td>
<td><pre><code>Id of the basket</code></pre></td>
</tr>
<tr>
<td><pre><code>userId</code></pre></td>
<td><pre><code>int</code></pre></td>
<td><pre><code>int</code></pre></td>
<td><pre><code>false</code></pre></td>
<td><pre><code>Id of the user</code></pre></td>
</tr>
<tr>
<td><pre><code>sku</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>true</code></pre></td>
<td><pre><code>Article number of the download</code></pre></td>
</tr>
<tr>
<td><pre><code>timeOfOrder</code></pre></td>
<td><pre><code>\DateTime</code></pre></td>
<td><pre><code>datetime</code></pre></td>
<td><pre><code>true</code></pre></td>
<td><pre><code>Timestamp of the order</code></pre></td>
</tr>
<tr>
<td><pre><code>quid</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>true</code></pre></td>
<td><pre><code>Unique hash 32 chars long</code></pre>
<pre><code>is created automatically</code></pre></td>
</tr>
</tbody>
</table>
