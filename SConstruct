import os

VERSION = '1.0.1'

env = Environment()

env['ENV']['PATH'] = os.environ['PATH']

def CheckPKGConfig(context, version):
    context.Message( 'Checking for pkg-config... ' )
    ret = context.TryAction('pkg-config --atleast-pkgconfig-version=%s' % version)[0]
    context.Result(ret)
    return ret

def CheckPKG(context, name):
    context.Message('Checking for %s... ' % name)
    ret = context.TryAction("pkg-config --exists '%s'" % name)[0]
    context.Result(ret)
    return ret

if (not GetOption('clean')):
    conf = env.Configure(custom_tests = { 'CheckPKGConfig' : CheckPKGConfig,
                                            'CheckPKG' : CheckPKG })
    if not conf.CheckPKGConfig('0.15'):
        print 'ERROR: pkg-config not found.'
        Exit(1)

    _LIBS = ['liblo']

    for lib in _LIBS:
        if not conf.CheckPKG(lib):
            print "ERROR: '%s' must be installed!" % (lib)
            Exit(1)
        else:
            env.ParseConfig('pkg-config --cflags --static --libs %s' % (lib))

    env = conf.Finish()

env.Append(CFLAGS=' -DVERSION=\\"' + VERSION +'\\"')
env.Program('oscdump', ['oscdump.c'])
env.Program('oscsend', ['oscsend.c'])

