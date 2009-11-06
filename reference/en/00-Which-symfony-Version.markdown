Which symfony Version?
======================

This book has been written for both symfony 1.3 and symfony 1.4. As writing a
single book for two different versions of a software is quite unusual, this
section explains what are the main differences between the two versions, and
how to make the best choice for your projects.

Both symfony 1.3 and symfony 1.4 versions have been released at about the same
time (at the end of 2009). As a matter of fact, they both have the **exact
same feature set**. The only difference between the two versions is on how
they support backward compatibility with older symfony versions.

Symfony 1.3 is the release you want to use if you need to upgrade a legacy
project that uses an older symfony version (1.0, 1.1, or 1.2). It has a
backward compatibility layer and all the features that have been deprecated
during the 1.3 development period are still available. It means that upgrading
is easy, simple, and safe.

But if you start a new project today, you should use symfony 1.4. This version
has the same feature set as symfony 1.3 but all the deprecated features and
the compatibility layer has been removed. This version is cleaner and also a
bit faster than symfony 1.3. Another big advantage of using symfony 1.4 is its
longer support. Being a Long Term Support release, it is maintained by the
symfony core team for three years (until November 2012).

Of course, you can migrate your projects to symfony 1.3 and then slowly update
your code to remove the deprecated features and eventually move to symfony 1.4
to benefit from the long term support. You have plenty of time to plan the
move as symfony 1.3 is supported for a year (until November 2010).

As this book does not describe deprecated features, all examples work equally
well on both versions.
