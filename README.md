Django REST framework tutorial
************
    runserver
    * bash: curl -H 'Accept: application/json; indent=4' -au admin:123456 http://127.0.0.1:8000/users/
    
    
    * sudo apt  install httpie
    * bash: http -a admin:password123 http://127.0.0.1:8000/users/
    
    Or directly through the browser, by going to the URL http://127.0.0.1:8000/users/...
    
**********
    pip install django
    pip install djangorestframework
    pip install pygments  # We'll be using this for the code highlighting
    
************
Serilizators
    
    >>> from snippets.models import Snippet
    >>> from snippets.serializers import SnippetSerializer
    >>> from rest_framework.renderers import JSONRenderer
    >>> from rest_framework.parsers import JSONParser
    
    >>> snippet = Snippet(code='foo = "bar"\n')
    >>> snippet.save()
    >>> snippet = Snippet(code='print("hello, world")\n')
    >>> snippet.save()
    
    # render the data into json.
    >>> serializer = SnippetSerializer(snippet)
    >>>  serializer.data
    {'id': 2, 'title': '', 'code': 'print("hello, world")\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
    >>> content = JSONRenderer().render(serializer.data)
    >>> content
    b'{"id":2,"title":"","code":"print(\\"hello, world\\")\\n","linenos":false,"language":"python","style":"friendly"}'

    # Deserialization
    >>> import io
    >>> stream = io.BytesIO(content)
    >>> data = JSONParser().parse(stream)
    >>> serializer = SnippetSerializer(data=data)
    >>> serializer.is_valid()
    True
    >>> serializer.validated_data
    OrderedDict([('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
    >>> serializer.save()
    <Snippet: Snippet object (3)>

    # serialize querysets instead of model instances
    >>> serializer = SnippetSerializer(Snippet.objects.all(), many=True)
    >>> serializer.data
    [OrderedDict([('id', 1), ('title', ''), ('code', 'foo = "bar"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 2), ('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 3), ('title', ''), ('code', 'print("hello, world")'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])]

    # model serializer
    >>> from snippets.serializers import SnippetSerializer
    >>> serializer = SnippetSerializer()
    >>> print(repr(serializer))
    SnippetSerializer():
        id = IntegerField(read_only=True)
        title = CharField(allow_blank=True, max_length=100, required=False)
        code = CharField(style={'base_template': 'textarea.html'})
        linenos = BooleanField(required=False)
        language = ChoiceField(choices=[('abap', 'ABAP'), ('abnf', 'ABNF'), ('ada', 'Ada'), ('adl', 'ADL'), ('agda', 'Agda'), ('aheui', 'Aheui'), ('ahk', 'autohotkey'), ('alloy', 'Alloy'), ('ampl', 'Ampl'), ('antlr', 'ANTLR'), ('antlr-as', 'ANTLR With ActionScript Target'), ('antlr-cpp', 'ANTLR With CPP Target'), ('antlr-csharp', 'ANTLR With C# Target'), ('antlr-java', 'ANTLR With Java Target'), ('antlr-objc', 'ANTLR With ObjectiveC Target'), ('antlr-perl', 'ANTLR With Perl Target'), ('antlr-python', 'ANTLR With Python Target'), ('antlr-ruby', 'ANTLR With Ruby Target'), ('apacheconf', 'ApacheConf'), ('apl', 'APL'), ('applescript', 'AppleScript'), ('arduino', 'Arduino'), ('arrow', 'Arrow'), ('as', 'ActionScript'), ('as3', 'ActionScript 3'), ('aspectj', 'AspectJ'), ('aspx-cs', 'aspx-cs'), ('aspx-vb', 'aspx-vb'), ('asy', 'Asymptote'), ('at', 'AmbientTalk'), ('augeas', 'Augeas'), ('autoit', 'AutoIt'), ('awk', 'Awk'), ('bare', 'BARE'), ('basemake', 'Base Makefile'), ('bash', 'Bash'), ('bat', 'Batchfile'), ('bbcbasic', 'BBC Basic'), ('bbcode', 'BBCode'), ('bc', 'BC'), ('befunge', 'Befunge'), ('bib', 'BibTeX'), ('blitzbasic', 'BlitzBasic'), ('blitzmax', 'BlitzMax'), ('bnf', 'BNF'), ('boa', 'Boa'), ('boo', 'Boo'), ('boogie', 'Boogie'), ('brainfuck', 'Brainfuck'), ('bst', 'BST'), ('bugs', 'BUGS'), ('c', 'C'), ('c-objdump', 'c-objdump'), ('ca65', 'ca65 assembler'), ('cadl', 'cADL'), ('camkes', 'CAmkES'), ('capdl', 'CapDL'), ('capnp', "Cap'n Proto"), ('cbmbas', 'CBM BASIC V2'), ('ceylon', 'Ceylon'), ('cfc', 'Coldfusion CFC'), ('cfengine3', 'CFEngine3'), ('cfm', 'Coldfusion HTML'), ('cfs', 'cfstatement'), ('chai', 'ChaiScript'), ('chapel', 'Chapel'), ('charmci', 'Charmci'), ('cheetah', 'Cheetah'), ('cirru', 'Cirru'), ('clay', 'Clay'), ('clean', 'Clean'), ('clojure', 'Clojure'), ('clojurescript', 'ClojureScript'), ('cmake', 'CMake'), ('cobol', 'COBOL'), ('cobolfree', 'COBOLFree'), ('coffee-script', 'CoffeeScript'), ('common-lisp', 'Common Lisp'), ('componentpascal', 'Component Pascal'), ('console', 'Bash Session'), ('control', 'Debian Control file'), ('coq', 'Coq'), ('cpp', 'C++'), ('cpp-objdump', 'cpp-objdump'), ('cpsa', 'CPSA'), ('cr', 'Crystal'), ('crmsh', 'Crmsh'), ('croc', 'Croc'), ('cryptol', 'Cryptol'), ('csharp', 'C#'), ('csound', 'Csound Orchestra'), ('csound-document', 'Csound Document'), ('csound-score', 'Csound Score'), ('css', 'CSS'), ('css+django', 'CSS+Django/Jinja'), ('css+erb', 'CSS+Ruby'), ('css+genshitext', 'CSS+Genshi Text'), ('css+lasso', 'CSS+Lasso'), ('css+mako', 'CSS+Mako'), ('css+mozpreproc', 'CSS+mozpreproc'), ('css+myghty', 'CSS+Myghty'), ('css+php', 'CSS+PHP'), ('css+smarty', 'CSS+Smarty'), ('cucumber', 'Gherkin'), ('cuda', 'CUDA'), ('cypher', 'Cypher'), ('cython', 'Cython'), ('d', 'D'), ('d-objdump', 'd-objdump'), ('dart', 'Dart'), ('dasm16', 'DASM16'), ('delphi', 'Delphi'), ('devicetree', 'Devicetree'), ('dg', 'dg'), ('diff', 'Diff'), ('django', 'Django/Jinja'), ('docker', 'Docker'), ('doscon', 'MSDOS Session'), ('dpatch', 'Darcs Patch'), ('dtd', 'DTD'), ('duel', 'Duel'), ('dylan', 'Dylan'), ('dylan-console', 'Dylan session'), ('dylan-lid', 'DylanLID'), ('earl-grey', 'Earl Grey'), ('easytrieve', 'Easytrieve'), ('ebnf', 'EBNF'), ('ec', 'eC'), ('ecl', 'ECL'), ('eiffel', 'Eiffel'), ('elixir', 'Elixir'), ('elm', 'Elm'), ('emacs', 'EmacsLisp'), ('email', 'E-mail'), ('erb', 'ERB'), ('erl', 'Erlang erl session'), ('erlang', 'Erlang'), ('evoque', 'Evoque'), ('execline', 'execline'), ('extempore', 'xtlang'), ('ezhil', 'Ezhil'), ('factor', 'Factor'), ('fan', 'Fantom'), ('fancy', 'Fancy'), ('felix', 'Felix'), ('fennel', 'Fennel'), ('fish', 'Fish'), ('flatline', 'Flatline'), ('floscript', 'FloScript'), ('forth', 'Forth'), ('fortran', 'Fortran'), ('fortranfixed', 'FortranFixed'), ('foxpro', 'FoxPro'), ('freefem', 'Freefem'), ('fsharp', 'F#'), ('fstar', 'FStar'), ('gap', 'GAP'), ('gas', 'GAS'), ('gdscript', 'GDScript'), ('genshi', 'Genshi'), ('genshitext', 'Genshi Text'), ('glsl', 'GLSL'), ('gnuplot', 'Gnuplot'), ('go', 'Go'), ('golo', 'Golo'), ('gooddata-cl', 'GoodData-CL'), ('gosu', 'Gosu'), ('groff', 'Groff'), ('groovy', 'Groovy'), ('gst', 'Gosu Template'), ('haml', 'Haml'), ('handlebars', 'Handlebars'), ('haskell', 'Haskell'), ('haxeml', 'Hxml'), ('hexdump', 'Hexdump'), ('hlsl', 'HLSL'), ('hsail', 'HSAIL'), ('hspec', 'Hspec'), ('html', 'HTML'), ('html+cheetah', 'HTML+Cheetah'), ('html+django', 'HTML+Django/Jinja'), ('html+evoque', 'HTML+Evoque'), ('html+genshi', 'HTML+Genshi'), ('html+handlebars', 'HTML+Handlebars'), ('html+lasso', 'HTML+Lasso'), ('html+mako', 'HTML+Mako'), ('html+myghty', 'HTML+Myghty'), ('html+ng2', 'HTML + Angular2'), ('html+php', 'HTML+PHP'), ('html+smarty', 'HTML+Smarty'), ('html+twig', 'HTML+Twig'), ('html+velocity', 'HTML+Velocity'), ('http', 'HTTP'), ('hx', 'Haxe'), ('hybris', 'Hybris'), ('hylang', 'Hy'), ('i6t', 'Inform 6 template'), ('icon', 'Icon'), ('idl', 'IDL'), ('idris', 'Idris'), ('iex', 'Elixir iex session'), ('igor', 'Igor'), ('inform6', 'Inform 6'), ('inform7', 'Inform 7'), ('ini', 'INI'), ('io', 'Io'), ('ioke', 'Ioke'), ('irc', 'IRC logs'), ('isabelle', 'Isabelle'), ('j', 'J'), ('jags', 'JAGS'), ('jasmin', 'Jasmin'), ('java', 'Java'), ('javascript+mozpreproc', 'Javascript+mozpreproc'), ('jcl', 'JCL'), ('jlcon', 'Julia console'), ('js', 'JavaScript'), ('js+cheetah', 'JavaScript+Cheetah'), ('js+django', 'JavaScript+Django/Jinja'), ('js+erb', 'JavaScript+Ruby'), ('js+genshitext', 'JavaScript+Genshi Text'), ('js+lasso', 'JavaScript+Lasso'), ('js+mako', 'JavaScript+Mako'), ('js+myghty', 'JavaScript+Myghty'), ('js+php', 'JavaScript+PHP'), ('js+smarty', 'JavaScript+Smarty'), ('jsgf', 'JSGF'), ('json', 'JSON'), ('json-object', 'JSONBareObject'), ('jsonld', 'JSON-LD'), ('jsp', 'Java Server Page'), ('julia', 'Julia'), ('juttle', 'Juttle'), ('kal', 'Kal'), ('kconfig', 'Kconfig'), ('kmsg', 'Kernel log'), ('koka', 'Koka'), ('kotlin', 'Kotlin'), ('lagda', 'Literate Agda'), ('lasso', 'Lasso'), ('lcry', 'Literate Cryptol'), ('lean', 'Lean'), ('less', 'LessCss'), ('lhs', 'Literate Haskell'), ('lidr', 'Literate Idris'), ('lighty', 'Lighttpd configuration file'), ('limbo', 'Limbo'), ('liquid', 'liquid'), ('live-script', 'LiveScript'), ('llvm', 'LLVM'), ('llvm-mir', 'LLVM-MIR'), ('llvm-mir-body', 'LLVM-MIR Body'), ('logos', 'Logos'), ('logtalk', 'Logtalk'), ('lsl', 'LSL'), ('lua', 'Lua'), ('make', 'Makefile'), ('mako', 'Mako'), ('maql', 'MAQL'), ('mask', 'Mask'), ('mason', 'Mason'), ('mathematica', 'Mathematica'), ('matlab', 'Matlab'), ('matlabsession', 'Matlab session'), ('md', 'markdown'), ('mime', 'MIME'), ('minid', 'MiniD'), ('modelica', 'Modelica'), ('modula2', 'Modula-2'), ('monkey', 'Monkey'), ('monte', 'Monte'), ('moocode', 'MOOCode'), ('moon', 'MoonScript'), ('mosel', 'Mosel'), ('mozhashpreproc', 'mozhashpreproc'), ('mozpercentpreproc', 'mozpercentpreproc'), ('mql', 'MQL'), ('ms', 'MiniScript'), ('mscgen', 'Mscgen'), ('mupad', 'MuPAD'), ('mxml', 'MXML'), ('myghty', 'Myghty'), ('mysql', 'MySQL'), ('nasm', 'NASM'), ('ncl', 'NCL'), ('nemerle', 'Nemerle'), ('nesc', 'nesC'), ('newlisp', 'NewLisp'), ('newspeak', 'Newspeak'), ('ng2', 'Angular2'), ('nginx', 'Nginx configuration file'), ('nim', 'Nimrod'), ('nit', 'Nit'), ('nixos', 'Nix'), ('notmuch', 'Notmuch'), ('nsis', 'NSIS'), ('numpy', 'NumPy'), ('nusmv', 'NuSMV'), ('objdump', 'objdump'), ('objdump-nasm', 'objdump-nasm'), ('objective-c', 'Objective-C'), ('objective-c++', 'Objective-C++'), ('objective-j', 'Objective-J'), ('ocaml', 'OCaml'), ('octave', 'Octave'), ('odin', 'ODIN'), ('ooc', 'Ooc'), ('opa', 'Opa'), ('openedge', 'OpenEdge ABL'), ('pacmanconf', 'PacmanConf'), ('pan', 'Pan'), ('parasail', 'ParaSail'), ('pawn', 'Pawn'), ('peg', 'PEG'), ('perl', 'Perl'), ('perl6', 'Perl6'), ('php', 'PHP'), ('pig', 'Pig'), ('pike', 'Pike'), ('pkgconfig', 'PkgConfig'), ('plpgsql', 'PL/pgSQL'), ('pointless', 'Pointless'), ('pony', 'Pony'), ('postgresql', 'PostgreSQL SQL dialect'), ('postscript', 'PostScript'), ('pot', 'Gettext Catalog'), ('pov', 'POVRay'), ('powershell', 'PowerShell'), ('praat', 'Praat'), ('prolog', 'Prolog'), ('promql', 'PromQL'), ('properties', 'Properties'), ('protobuf', 'Protocol Buffer'), ('ps1con', 'PowerShell Session'), ('psql', 'PostgreSQL console (psql)'), ('psysh', 'PsySH console session for PHP'), ('pug', 'Pug'), ('puppet', 'Puppet'), ('py2tb', 'Python 2.x Traceback'), ('pycon', 'Python console session'), ('pypylog', 'PyPy Log'), ('pytb', 'Python Traceback'), ('python', 'Python'), ('python2', 'Python 2.x'), ('qbasic', 'QBasic'), ('qml', 'QML'), ('qvto', 'QVTO'), ('racket', 'Racket'), ('ragel', 'Ragel'), ('ragel-c', 'Ragel in C Host'), ('ragel-cpp', 'Ragel in CPP Host'), ('ragel-d', 'Ragel in D Host'), ('ragel-em', 'Embedded Ragel'), ('ragel-java', 'Ragel in Java Host'), ('ragel-objc', 'Ragel in Objective C Host'), ('ragel-ruby', 'Ragel in Ruby Host'), ('raw', 'Raw token data'), ('rb', 'Ruby'), ('rbcon', 'Ruby irb session'), ('rconsole', 'RConsole'), ('rd', 'Rd'), ('reason', 'ReasonML'), ('rebol', 'REBOL'), ('red', 'Red'), ('redcode', 'Redcode'), ('registry', 'reg'), ('resource', 'ResourceBundle'), ('rexx', 'Rexx'), ('rhtml', 'RHTML'), ('ride', 'Ride'), ('rnc', 'Relax-NG Compact'), ('roboconf-graph', 'Roboconf Graph'), ('roboconf-instances', 'Roboconf Instances'), ('robotframework', 'RobotFramework'), ('rql', 'RQL'), ('rsl', 'RSL'), ('rst', 'reStructuredText'), ('rts', 'TrafficScript'), ('rust', 'Rust'), ('sarl', 'SARL'), ('sas', 'SAS'), ('sass', 'Sass'), ('sc', 'SuperCollider'), ('scala', 'Scala'), ('scaml', 'Scaml'), ('scdoc', 'scdoc'), ('scheme', 'Scheme'), ('scilab', 'Scilab'), ('scss', 'SCSS'), ('sgf', 'SmartGameFormat'), ('shen', 'Shen'), ('shexc', 'ShExC'), ('sieve', 'Sieve'), ('silver', 'Silver'), ('singularity', 'Singularity'), ('slash', 'Slash'), ('slim', 'Slim'), ('slurm', 'Slurm'), ('smali', 'Smali'), ('smalltalk', 'Smalltalk'), ('smarty', 'Smarty'), ('sml', 'Standard ML'), ('snobol', 'Snobol'), ('snowball', 'Snowball'), ('solidity', 'Solidity'), ('sourceslist', 'Debian Sourcelist'), ('sp', 'SourcePawn'), ('sparql', 'SPARQL'), ('spec', 'RPMSpec'), ('splus', 'S'), ('sql', 'SQL'), ('sqlite3', 'sqlite3con'), ('squidconf', 'SquidConf'), ('ssp', 'Scalate Server Page'), ('stan', 'Stan'), ('stata', 'Stata'), ('swift', 'Swift'), ('swig', 'SWIG'), ('systemverilog', 'systemverilog'), ('tads3', 'TADS 3'), ('tap', 'TAP'), ('tasm', 'TASM'), ('tcl', 'Tcl'), ('tcsh', 'Tcsh'), ('tcshcon', 'Tcsh Session'), ('tea', 'Tea'), ('termcap', 'Termcap'), ('terminfo', 'Terminfo'), ('terraform', 'Terraform'), ('tex', 'TeX'), ('text', 'Text only'), ('thrift', 'Thrift'), ('tid', 'tiddler'), ('tnt', 'Typographic Number Theory'), ('todotxt', 'Todotxt'), ('toml', 'TOML'), ('trac-wiki', 'MoinMoin/Trac Wiki markup'), ('treetop', 'Treetop'), ('ts', 'TypeScript'), ('tsql', 'Transact-SQL'), ('ttl', 'Tera Term macro'), ('turtle', 'Turtle'), ('twig', 'Twig'), ('typoscript', 'TypoScript'), ('typoscriptcssdata', 'TypoScriptCssData'), ('typoscripthtmldata', 'TypoScriptHtmlData'), ('ucode', 'ucode'), ('unicon', 'Unicon'), ('urbiscript', 'UrbiScript'), ('usd', 'USD'), ('vala', 'Vala'), ('vb.net', 'VB.net'), ('vbscript', 'VBScript'), ('vcl', 'VCL'), ('vclsnippets', 'VCLSnippets'), ('vctreestatus', 'VCTreeStatus'), ('velocity', 'Velocity'), ('verilog', 'verilog'), ('vgl', 'VGL'), ('vhdl', 'vhdl'), ('vim', 'VimL'), ('wdiff', 'WDiff'), ('webidl', 'Web IDL'), ('whiley', 'Whiley'), ('x10', 'X10'), ('xml', 'XML'), ('xml+cheetah', 'XML+Cheetah'), ('xml+django', 'XML+Django/Jinja'), ('xml+erb', 'XML+Ruby'), ('xml+evoque', 'XML+Evoque'), ('xml+lasso', 'XML+Lasso'), ('xml+mako', 'XML+Mako'), ('xml+myghty', 'XML+Myghty'), ('xml+php', 'XML+PHP'), ('xml+smarty', 'XML+Smarty'), ('xml+velocity', 'XML+Velocity'), ('xorg.conf', 'Xorg'), ('xquery', 'XQuery'), ('xslt', 'XSLT'), ('xtend', 'Xtend'), ('xul+mozpreproc', 'XUL+mozpreproc'), ('yaml', 'YAML'), ('yaml+jinja', 'YAML+Jinja'), ('yang', 'YANG'), ('zeek', 'Zeek'), ('zephir', 'Zephir'), ('zig', 'Zig')], default='python')
        style = ChoiceField(choices=[('abap', 'abap'), ('algol', 'algol'), ('algol_nu', 'algol_nu'), ('arduino', 'arduino'), ('autumn', 'autumn'), ('borland', 'borland'), ('bw', 'bw'), ('colorful', 'colorful'), ('default', 'default'), ('emacs', 'emacs'), ('friendly', 'friendly'), ('fruity', 'fruity'), ('igor', 'igor'), ('inkpot', 'inkpot'), ('lovelace', 'lovelace'), ('manni', 'manni'), ('monokai', 'monokai'), ('murphy', 'murphy'), ('native', 'native'), ('paraiso-dark', 'paraiso-dark'), ('paraiso-light', 'paraiso-light'), ('pastie', 'pastie'), ('perldoc', 'perldoc'), ('rainbow_dash', 'rainbow_dash'), ('rrt', 'rrt'), ('sas', 'sas'), ('solarized-dark', 'solarized-dark'), ('solarized-light', 'solarized-light'), ('stata', 'stata'), ('stata-dark', 'stata-dark'), ('stata-light', 'stata-light'), ('tango', 'tango'), ('trac', 'trac'), ('vim', 'vim'), ('vs', 'vs'), ('xcode', 'xcode')], default='friendly')

    # It's important to remember that ModelSerializer classes don't do anything particularly magical, they are simply a shortcut for creating serializer classes:
    #   An automatically determined set of fields.
    #   Simple default implementations for the create() and update() methods.

   
***********************
Requests and Responses

    request.POST  # Only handles form data.  Only works for 'POST' method.
    request.data  # Handles arbitrary data.  Works for 'POST', 'PUT' and 'PATCH' methods.
    return Response(data)  # Renders to content type as requested by the client.
    
    The @api_view decorator for working with function based views.
    The APIView class for working with class-based views.
    
    in app urls:    urlpatterns = format_suffix_patterns(urlpatterns)

    $ http http://127.0.0.1:8000/snippets/ Accept:application/json  # Request JSON
    $ http http://127.0.0.1:8000/snippets/ Accept:text/html         # Request HTML

    $ http http://127.0.0.1:8000/snippets.json  # JSON suffix
    $ http http://127.0.0.1:8000/snippets.api   # Browsable API suffix

    # POST using form data
    $ http --form POST http://127.0.0.1:8000/snippets/ code="print(123)"
    # POST using JSON
    $ http --json POST http://127.0.0.1:8000/snippets/ code="print(456)"

******************************
Class-based Views
    
    Class-based Views without mixins
    Class-based Views with mixins
    Class-based Views with generic class-based views

***************************
Authentication & Permissions

    In code:        
        Code snippets are always associated with a creator.
        Only authenticated users may create snippets.
        Only the creator of a snippet may update or delete it.
        Unauthenticated requests should have full read-only access.

    changes in model and:
            rm -f db.sqlite3
            rm -r snippets/migrations
            python manage.py makemigrations snippets
            python manage.py migrate
    
    $ http POST http://127.0.0.1:8000/snippets/ code="print(123)"

        {
            "detail": "Authentication credentials were not provided."
        }
        
    $ http -a admin:53402505 POST http://127.0.0.1:8000/snippets/ code="print(789)"
    success!
    $ curl -H 'Accept: application/json; indent=4' -au admin:53402505 http://127.0.0.1:8000/snippets/
    success!
    
    
    
    