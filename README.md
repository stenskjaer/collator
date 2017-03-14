Collator is a simple and early script that assists in collating an arbitrary
number of TEI XML transcriptions of a text. It uses the collation features
provided by [CollateX](https://collatex.net/) and creates a HTML document with a
representation of the witnesses that is inspired by the CollateX HTML output.

It is basically a wrapper for the CollateX CLI. It converts the witnesses into
plain text with a very small xslt-script (and therefore also uses saxon -- maybe
I could cut that dependency). It then reads those witnesses into a JSON
temporary file that it feeds to CollateX which returns a nested list that it
processes into a HTML representation.

This is developed to handle [LombardPress
Schema](http://lombardpress.org/schema/docs/) compliant material, but it might
handle many other TEI documents well for now, as the encoding conventions of the
document are not central to it.

# Installation

## Requirements

- Python 3.6 (you might as well get that updated anyway)
- Saxon HE
- CollateX (binary included in the repository)

### Saxon HE

On macOS, with [homebrew](https://brew.sh/):
``` bash
$ brew install saxon
```

You can also [download
it](https://sourceforge.net/projects/saxon/files/Saxon-HE/) and install it on
your own.

### Python packages

The only external dependency right now is the wonderful [docopt
module](http://docopt.org/). 

You should probably create a [virtual
environment](http://docs.python-guide.org/en/latest/dev/virtualenvs/) for the
project. To do that, run:

```bash
$ mkvirtualenv -p python3 <name>
```
Where `<name>` is the name you want to give the venv.

After activating the venv (`workon` or `source`), install dependencies:
```bash
$ pip install -r requirements.txt
```

## Usage

To see some quick results, run:

``` bash
$ ./collator.py examples/bal311_da-49-l1prooemium.xml examples/oriel33_da-49-l1prooemium.xml
```
It will result in the html that is already in the `examples`-directory. 

The usage statement:
``` 
Usage: collator.py [options] <file> <file>...

A script for simplifying collation of several text witnesses encoded according
to the Lombard Press Schema.

Arguments:
  <file> <file>...        Two or more files that are to be collated.

Options:
  -o, --output <file>     Location of the output file. [default: ./output.html].
  -V, --verbosity <level> Set verbosity. Possibilities: silent, info, debug [default: info].
  -v, --version           Show version and exit.
  -h, --help              Show this help message and exit.
```

The input files must be XML files. They will be converted to plain text during
processing. The following elements will be preserved in the plain text for later
analysis:
- unclear
- pb
- supplied
- secl
- del
- add

When a word is normalized with
`<choice><orig>sicud</orig><reg>sicut</reg></choice>`, the regularized form is used.

# Warning

The script is volatile. Anything may be subject to change, and I provide no
warranties for the safety of your texts, computer equipment or soul when using
the script.
