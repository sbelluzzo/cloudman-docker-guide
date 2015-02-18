---
title: About Project
layout: page
---
This work was the basis of a [VLSCI Summer Internship](http://vlsci.org.au/opportunities/internships)
project, conducted by Simon Belluzzo, supervised by Dr Ira Cooke, and supported by the
[VLSCI](http://www.vlsci.org.au).

Slides from the presentation of this work at the end of the internship program
can be found [here](http://simon.belluzzo.id.au/vlsci-intern-slides).

---

## Project Background
Ira had previously written [ProtK](https://github.com/iracooke/protk), a unifying and convenience wrapper around a set of proteomics tools, along with a set of tool wrappers to allow use of the tools in [Galaxy](http://galaxyproject.org/).
However, the complexity of installing the underlying tools and their dependencies, and ensuring interoperability with a Galaxy server, made installation by novice users difficult, especially in the context of [Galaxy on the Cloud](https://wiki.galaxyproject.org/CloudMan) or [Genomics Virtual Lab (GVL)](https://genome.edu.au/wiki/GVL) instances.
Therefore, the primary goal of this work was to explore the feasibility of using [Docker](http://www.docker.com/), a lightweight "jail"/"container" platform that has preliminary support in Galaxy, as a way of easily distributing and installing the tools along with required dependencies. 

## Project Aims
- Build a ProtK proteomics Docker image.
- Adapt Galaxy wrappers to utilise Docker image.
- Investigate deployment options.
- Set up for integration with GVL
  - Installation
  - Interaction with job runners
