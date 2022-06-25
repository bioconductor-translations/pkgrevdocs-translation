# The NEWS file

A `NEWS` file should be included to keep track of changes to the code from one version to the next. It can be a top level file or in the `inst/` directory. Only one `NEWS` file should exist in the repository.

The following are acceptable locations and formats:

<table>
<thead>
<tr class="header">
<th style="text-align: left;">location</th>
<th style="text-align: left;">format</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;"><code>./inst/NEWS.Rd</code></td>
<td style="text-align: left;"><span
class="math inline">$\LaTeX$</span></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>./inst/NEWS</code></td>
<td style="text-align: left;">formatted text see <code>?news</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>./inst/NEWS.md</code></td>
<td style="text-align: left;">markdown</td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>./NEWS.md</code></td>
<td style="text-align: left;">markdown</td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>./NEWS</code></td>
<td style="text-align: left;">formatted text see <code>?news</code></td>
</tr>
</tbody>
</table>

Specifics on formatting can be found on the help page for `?news`. \[*Bioconductor*\]\[\] uses the `NEWS` file to create the semi-annual release announcement. It must include list elements and **cannot** be a plain text file.

An example format:

    Changes in version 0.99.0 (2018-05-15)
    + Submitted to Bioconductor
    
    Changes in version 1.1.1 (2018-06-15)
    + Fixed bug. Begin indexing from 1 instead of 2
    + Made the following significant changes
      o added a subsetting method
      o added a new field to database

After you install your package, the following can be run to see if the `NEWS` is properly formatted:

    utils::news(package="<name of your package>")

The output should look similar to the following:

    Changes in version 1.1.1 (2018-06-15):
    
        o   Fixed bug. Begin indexing from 1 instead of 2
    
        o   Made the following significant changes
        o added a subsetting method
        o added a new field to database
    
    Changes in version 0.99.0 (2018-05-15):
    
        o   Submitted to Bioconductor

If you get something like the following there are formatting errors that need to be corrected:

    Version: 0.99.0
    Date: 2018-05-15
    Text: Submitted to Bioconductor
    
    Version: 1.1.1
    Date: 2018-06-15
    Text: Fixed bug. Begin indexing from 1 instead of 2
    
    Version: 1.1.1
    Date: 2018-06-15
    Text: Made the following significant changes o added a subsetting
        method o added a new field to database
