kaldi-io-for-python
===================
``Glue'' code connecting kaldi data and python.
-------------------

#### Supported data types
- vector (integer)
- Vector (float, double)
- Matrix (float, double)
- Posterior (posteriors, nnet1 training targets, confusion networks, ...)

#### Examples

###### Reading feature scp example:
```python
import kaldi_io
for key,mat in kaldi_io.read_mat_scp(file):
  ...
```

###### Writing feature ark to file/stream:
```python
import kaldi_io
with open(ark_file,'wb') as f:
  for key,mat in dict.iteritems(): 
    kaldi_io.write_mat(f, mat, key=key)
```

###### Writing features as 'ark,scp' by pipeline with 'copy-feats':
```python
import kaldi_io
ark_scp_output='ark:| copy-feats --compress=true ark:- ark,scp:data/feats2.ark,data/feats2.scp'
with kaldi_io.open_or_fd(ark_scp_output,'wb') as f:
  for key,mat in dict.iteritems(): 
    kaldi_io.write_mat(f, mat, key=key)
```

###### Writing to and reading from the pipe at the same time
```python
import kaldi_io
import soundfile as sf
cmd = "ark:| compute-mfcc-feats --config=/path/to/mfcc.conf ark:- ark:- |"
fin, fout = kio.open_or_fd(cmd) #note: mode here gets ignored
wav, rate = sf.read('test.wav')
kaldi_io.write_wav(fin, wav, rate, key='test')
fin.close() #so its clear nothing new arrives
feats_ark = kaldi_io.read_mat_ark(fout)
for utt, feats in feats_ark:
    #keep processing feats from Python
```

#### Install
- from pypi: `python -m pip --user install kaldi_io`
- from sources:
  - `git clone https://github.com/vesis84/kaldi-io-for-python.git <kaldi-io-dir>`
  - `python setup.py install` (default python)
- for local development use: `export PYTHONPATH=${PYTHONPATH}:<kaldi-io-dir>` in `$HOME/.bashrc`

Note: it is recommended to set `$KALDI_ROOT` in your `$HOME/.bashrc` as
`export KALDI_ROOT=<some_kaldi_dir>`, so you can read/write using 
pipes which contain kaldi binaries.


#### License
Apache License, Version 2.0 ('LICENSE-2.0.txt')

#### Contact
- If you have an extension to share, please create a pull request.
- For feedback and suggestions, please create a GitHub 'Issue' in the project.
- For the positive reactions =) I am also reachable by email: vesis84@gmail.com
