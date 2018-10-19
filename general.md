## General recommendations

This document is a compilation of general tips and suggestions that
doesn't fit anywhere ele or deserve their own documents.

### Documentation line width

We strongly recommend following the convention of keeping documentation lines
(docstrings, comments for modules, functions, methods, classes etc.) shorter
than 72 characters.

Please note that 72-character width cap targets only comment sections
(not code itself). It's much more readable if the natural language describing
a function is shorter than the code itself.

For the code we recommend 70-80 (see for example `python.md`).

### Technical Debt

We sometimes find ourselves in situations which require from us to make a
technical debt - be it quickly approaching deadline or critical outage that
must be fixed as soon as possible. Recognizing when it's really necessary to
make such decision is a valuable skill. However, regardless of the context,
there are rules that must be followed when making a debt. These are as follows:

1. Whenever you consciously leaving a code smell, you must document it. This
includes making an Issue/Ticket and leaving a comment in the code fragment.
Comment must describe what is being done, reference the Issue/Ticket and,
optimally, describe proper way to solve the problem in the near future.
For example:

```python
# FIXME Issue-34
# This is ugly, but it works and client needs a fix in 5.
# This should be refactored to use context manager.

lock = threading.Lock()
lock.acquire()
try:
    print 'Critical section 1'
    print 'Critical section 2'
finally:
    lock.release()
```

After refactoring:

```python
with lock:
    print 'Critical section 1'
    print 'Critical section 2'
```

The format is:

```python
FIXME <issue>
<comment>
```

Comment can also be in the first line.

2. Resolve the issue as soon as possible. Until it is fixed, original task
cannot be considered done. If you're a developer, it is your job to nag your
technical lead to allocate time for this. If you're a lead, it is your job
to see it done.
