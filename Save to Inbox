# coding: utf-8
"""
Pythonista script to run as an extension. Recieves file and save it to an Inbox folder. Unzips if neccessary.
Works for sending repositories from Working Copy to Pythonista.
"""
from shutil import copyfile, copytree, rmtree
import os
import time
import zipfile
try:
    import appex
except ImportError:
    exit('This script only works in Pythonista')

inboxpath = os.path.expanduser('~/Documents/Inbox')

if not os.path.isdir(inboxpath):
    os.makedirs(inboxpath)

if appex.is_running_extension():

    sources = appex.get_file_paths()
    for source in sources:

        destination = os.path.join(
            inboxpath,
            os.path.basename(source)
        )

        if os.path.isfile(source):
            if os.path.isfile(destination):
                os.remove(destination)
                print('Existing file at destination removed.')
            elif os.path.isdir(destination):
                rmtree(destination)
                print('Existing directory at destination removed.')

            copyfile(source, destination)
            print('%s > %s' % (os.path.basename(source), os.path.basename(inboxpath)))
            if destination[-4:].lower() == '.zip':
                unzippath = os.path.join(inboxpath, os.path.basename(destination).split('.')[0])
                if os.path.isdir(unzippath):
                    rmtree(unzippath)
                os.makedirs(unzippath)

                with zipfile.ZipFile(destination, 'r') as myzip:
                    myzip.extractall(unzippath)
                    os.remove(destination)
                print('Unzipped')
        elif os.path.isdir(source):
            copytree(source, destination)
            print('%s > %s' % (os.path.basename(source), os.path.basename(inboxpath)))

    time.sleep(1)
    appex.finish()
else:
    print('This script only runs as an extension.')
