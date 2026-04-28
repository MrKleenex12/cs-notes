# MongoDB Schema Design Best Practices

Here are my notes for Joe Karlsson's video on MongoDB Schema Design Best Practices.

## Embedding vs Referencing

![image](https://d36ai2hkxl16us.cloudfront.net/course-uploads/ae62dcd7-abdc-4e90-a570-83eccba49043/7ev1uxwmqacn-embeddingreferencing.png)

### Embedding

![embedding](https://velog.velcdn.com/images%2Fjavawork93%2Fpost%2Fc1f7279f-b653-41bd-b98d-c4bd95dd3c85%2Fembedding.png)

Embedding in documents means the document already contains all the data for the specific field in the document. This may
be an array of items or objects with multiple fields. This means retrieving all that data is *extremely simple*. There
is no need for joins or multiple queries to retrieve said data. It is already in the document. This also means that
updates to the data are atomic (updated in a single operation).

Some cons of embedding is that it *increases the size* of the document. Since the document already stores the data, it creates
more overhead for the query and retrevial. Larger files/documents means longer reads. Another point is that MongoDB has a
*16MB document size* limit.

### Referencing

![referencing](https://velog.velcdn.com/images%2Fjavawork93%2Fpost%2F7571b217-440a-435f-96c2-b35921058dd9%2Freferencing.png)

Referencing in documents means that the document stores an ID or fields that references the data somewhere else. This leads
to *smaller documents*. This means that it is much harder to reach the 16MB document size limit. Referencing also means that
there is *no duplication of data*. Normalization of data is a common practice in Relational DB Models. The last pro is that
the data is only retrieved when specifically needed, not all the time.

The only (main) con of referencing data is that now it requires two queries or $lookup to retrieve it all.

## Data Relationships

### One to One

Use key-value pairs. Simple.

### One to Few

Prefer embedding

### One to Many

Prefer Referencing

### One to Quillions?

This is where we are retreiving tons and tons of entries like logs

![oneToSquillians](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ffiles.speakerdeck.com%2Fpresentations%2F2b66714c97454b1e8855185d98e4c235%2Fslide_58.jpg&f=1&nofb=1&ipt=05b7690241fc8597579c279b9ddcf7e6ec6d82994737360aef02c2f2a239c517)

See how the log stores none of the host data. This could be super helpful in reducing the amount of variety of looks. Imagine that
there is one computer that stores the hosts and another that stores the logs. Instead of using the host computer to search all the
computers that relate back to it, the logs have a reference to the host commputer. This is the opposite of the host embedding all
the log ids.

Prefer Referencing

### Many to Many

This is where a list of actions/objects can have multiple owners.

![manyToMany](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmedia.licdn.com%2Fdms%2Fimage%2Fv2%2FD4D22AQGyAYGkjmn5ug%2Ffeedshare-shrink_2048_1536%2Ffeedshare-shrink_2048_1536%2F0%2F1666691626535%3Fe%3D2147483647%26v%3Dbeta%26t%3DqYvmQygnuu_kMLV2rkqwXk5L4V5lXmDe831Xl1T0ICQ&f=1&nofb=1&ipt=705187b021dcb0d218501ae14f8327f9b9fb10da36a6f2879f5f0660df79da38)

It is basically a two way reference.

## Rules

1. Favor embedding unless there is a compelling reason not to
2. Accessing an object on its own is a compelling reason not to embed it
3. Avoid JOINs and $lookups if can be, but don't be afraid if they can provide a better schema design
4. Arrays should not grow without bound
5. How you model your data depends - entirely - on your particular application's data access patterns.

## References

Karlsson, J. (2020, April 16). MongoDB Schema Design Best Practices. Youtube. <https://www.youtube.com/watch?v=leNCfU5SYR8>
