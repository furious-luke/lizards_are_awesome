# lizards-are-awesome

A Docker based workflow for performing a Plink/fastStructure analysis from Excel data.


## Overview

This software seeks to ease much the manual labour involved in preparing data for
running with Plink and fastStructure. Most of the work, besides the mentioned
external packages, is done with a Python script. The primary operations
performed by the script are:

 1. Duplicating the input data.
 2. Performing a substitution on certain values in the duplicated data.
 3. Indpenendantly indexing both sets of data.
 4. Combining the original and duplicated data.
 5. Sorting on the combined index.
 6. Transposing the combined data.
 7. Outputting to a Plink compatible format.

Whereas before these steps would be carred out manually using various software
packages, they are now performed automatically.

In addition to the main conversion operation, there are additional functions
to perform analysis runs of Plink and fastStructre, passing the data files
between the two programs automatically.


## Design Decisions

### Why Docker?

`Plink` is written for Linux based operating systems. As such on a Linux system
all operations could be performed directly, without the need for any kind of
virtualisation layer. But, in order to support researchers using Windows based
operating systems the decision was made to leverage Docker virtualisation.

Docker provides a light-weight virtualisation layer enabling Linux software to
run on Windows with (relative) ease. It also has the added benefit of providing
a cloud based mechanism for disseminating software "images" to users. The advantage
of Docker over other systems, like VirtualBox or VMWare, are:

 * cloud based distribution of prebuilt images,
 * future releases will allow native Docker containers, and
 * easy to replicate virtual image creation.

### Why Python?

Python is a powerful and expressive scripting language. It comes with many
diverse packages, and has excellent support from developers (for example,
`fasStructure` is written in Python).


## Dependencies

When installing on any platform there are number of requisite dependencies:

 * Python
 * Docker

If you happen to be installing on Windows, then there are a couple of extra requirements:

 * Visual Studio Python compiler
 * MsysGit


## Installation

Begin by installing all of the dependencies for your operating system as
listed above.

Once complete, open a system terminal (please see the subsection on system terminals
below, under `usage`).

From an open system terminal, install the LAA Python interface with:

```bash
pip install laa
```

Next, from a system terminal, download and prepare the `laa` docker image. This
image contains `plink`, `fastStructure`, and the conversion scripts, all built
into a light-weight Alpine linux image:

```bash
laa init
```

## Usage

### Terminals

Usage is currently done directly from your operating system terminal. In Linux
like operating systems (including Mac OS X) use the system terminal emulator. In
Windows operating systems use the Docker quick start terminal.

### Input Format

LAA accepts XLSX Excel formats and CSV. Unfortunately, XLSX is extremely slow
to parse using opensource utilities. As such we recommend converting your Excel
data to CSV before use with LAA (this is easy to do using Microsoft Office, or
opensource spreadsheet tools, like Libre Office).

The data may be given to LAA in either a recombined or raw format. The raw format
is more typical, as LAA handles recombination automatically, so we'll only discuss
the raw format.

The first two rows of the raw format should contain strings, defining the human
readable names of the columns, and the short-hand names of the columns. All following
rows contain the (??) data.

A short, completely fictitious, example:

<table class="table table-bordered table-hover table-condensed">
<tbody><tr><td>AA</td>
<td>AB</td>
<td>AC</td>
<td>AD</td>
<td>AE</td>
<td>AF</td>
<td>AG</td>
<td>AH</td>
<td>AI</td>
<td>AJ</td>
</tr>
<tr><td>aa</td>
<td>ab</td>
<td>ac</td>
<td>ad</td>
<td>ae</td>
<td>af</td>
<td>ag</td>
<td>ah</td>
<td>ai</td>
<td>aj</td>
</tr>
<tr><td>0</td>
<td>1</td>
<td>2</td>
<td>3</td>
<td>4</td>
<td>5</td>
<td>6</td>
<td>7</td>
<td>8</td>
<td>9</td>
</tr>
<tr><td>10</td>
<td>11</td>
<td>12</td>
<td>13</td>
<td>14</td>
<td>15</td>
<td>16</td>
<td>17</td>
<td>18</td>
<td>19</td>
</tr>
<tr><td>20</td>
<td>21</td>
<td>22</td>
<td>23</td>
<td>24</td>
<td>25</td>
<td>26</td>
<td>27</td>
<td>28</td>
<td>29</td>
</tr>
<tr><td>30</td>
<td>31</td>
<td>32</td>
<td>33</td>
<td>34</td>
<td>35</td>
<td>36</td>
<td>37</td>
<td>38</td>
<td>39</td>
</tr>
<tr><td>40</td>
<td>41</td>
<td>42</td>
<td>43</td>
<td>44</td>
<td>45</td>
<td>46</td>
<td>47</td>
<td>48</td>
<td>49</td>
</tr>
<tr><td>50</td>
<td>51</td>
<td>52</td>
<td>53</td>
<td>54</td>
<td>55</td>
<td>56</td>
<td>57</td>
<td>58</td>
<td>59</td>
</tr>
<tr><td>60</td>
<td>61</td>
<td>62</td>
<td>63</td>
<td>64</td>
<td>65</td>
<td>66</td>
<td>67</td>
<td>68</td>
<td>69</td>
</tr>
<tr><td>70</td>
<td>71</td>
<td>72</td>
<td>73</td>
<td>74</td>
<td>75</td>
<td>76</td>
<td>77</td>
<td>78</td>
<td>79</td>
</tr>
<tr><td>80</td>
<td>81</td>
<td>82</td>
<td>83</td>
<td>84</td>
<td>85</td>
<td>86</td>
<td>87</td>
<td>88</td>
<td>89</td>
</tr>
<tr><td>90</td>
<td>91</td>
<td>92</td>
<td>93</td>
<td>94</td>
<td>95</td>
<td>96</td>
<td>97</td>
<td>98</td>
<td>99</td>
</tr>
</tbody></table>

And, in CSV format:

```csv
AA,AB,AC,AD,AE,AF,AG,AH,AI,AJ
aa,ab,ac,ad,ae,af,ag,ah,ai,aj
0,1,2,3,4,5,6,7,8,9
10,11,12,13,14,15,16,17,18,19
20,21,22,23,24,25,26,27,28,29
30,31,32,33,34,35,36,37,38,39
40,41,42,43,44,45,46,47,48,49
50,51,52,53,54,55,56,57,58,59
60,61,62,63,64,65,66,67,68,69
70,71,72,73,74,75,76,77,78,79
80,81,82,83,84,85,86,87,88,89
90,91,92,93,94,95,96,97,98,99
```

### Location

All LAA commands must be run from the same directory you have your CSV input file
in. For the purpose of the examples, let's say we have an input file, `input.csv`,
located at `/c/workspace/data`:

```bash
cd /c/workspace/data
```

### Quick-run

To perform the complete process, including conversion, Plink, fastStructre and
analysing for K values, you can just run:

```bash
laa all input.csv --maxk=5
```

where `--maxk=5` may be replaced with a suitable value for the maximum K value to
use.

This will produce a range of files in the current working directory corresponding
to the outputs of the conversion, Plink, and fastStructre.

### Conversion

Converting the input data will peform recombination, transposition, output
to a PED file, and also generation of a suitable mapping file:

```bash
laa convert input.csv output.ped
```

This will generate two files: `output.ped`, and `output.map`. These files are
suitable for use with Plink.

### Plink

To process the converted input files with Plink, run:

```bash
laa plink output.ped
```

### fastStructure

To process the Plink outputs with fastStructure, run:

```bash
laa fast output
```

### K Choice

To run fastStructure a number of times, and then choose an appropriate
K value, run:

```bash
laa choosek output --maxk=5
```

where `--maxk=5` may be replaced with a suitable value for the maximum K value to
use.

## Getting Help

Help is always available from the command-line. To get a printout of available commands,
run:

```bash
laa -h
```

You may also get help for a specific command with something like:

```bash
laa convert -h
```
