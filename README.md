# PGP_key_Verification

# Verifying downloaded software

The webpage that propelled this investigation and summarization was [ThisOne](https://www.apache.org/info/verification.html#verifying-apache-software-foundation-releases)

## Verify that the downloaded file was signed by the downloaded key

`gpg --verify apache-maven-3.9.6.zip.asc apache-maven-3.9.6-bin.zip`

### Checking the output

```
% gpg --verify httpd-2.0.44.tar.gz.asc httpd-2.0.44.tar.gz
gpg: Signature made Sat Jan 18 07:21:28 2003 PST using DSA key ID DE885DD3
gpg: Can't check signature: public key not found
```

An output such as this means the key downloaded was really used to sign the file. BUT is the key registered with a certificate authority??

## Download the public key related to the downloaded key

```
% gpg --keyserver pgpkeys.mit.edu --recv-key DE885DD3
gpg: requesting key DE885DD3 from HKP keyserver pgpkeys.mit.edu
gpg: trustdb created
gpg: key DE885DD3: public key "Sander Striker <striker@apache.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

## With the public key downloaded we can check the file again

```
% gpg --verify httpd-2.0.44.tar.gz.asc httpd-2.0.44.tar.gz
gpg: Signature made Sat Jan 18 07:21:28 2003 PST using DSA key ID DE885DD3
gpg: Good signature from "Sander Striker <striker@apache.org>"
gpg:                 aka "Sander Striker <striker@striker.nl>"
gpg: checking the trustdb
gpg: no ultimately trusted keys found
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Fingerprint: 4C1E ADAD B4EF 5007 579C  919C 6635 B6C0 DE88 5DD3
```

This means the key used to sign the file we downloaded IS really registered with a CertificateAuthority, BUT does not mean the name in the key, in this case ~Sander Striker~ is real, in other words, does not mean we should thrust the person who registered the key.

## Verifying via uploaded photography.

The PGP infrastructure allows one to upload a `.png` or `.jpg` file with a picture, to provide some ID on the signature.

## Command to download the ID image associated with a key

The command was found on [ThisPost](https://stackoverflow.com/questions/54028532/how-to-extract-image-from-pgp-key-on-command-line)

```
To export the image, I haven't found any simpler way than cat the data (if %i isn't specified, gpg sends image data on stdin) to a file like this:
(%k - keyID; %t - extension --> filenames like 0x02468ACE.jpg)

gpg --list-options show-photos --photo-viewer "cat > <path>/0x%k.%t" --list-keys [key_identifier]

```

Example of the command on a gitBash command line:

```
gpg --list-options show-photos --photo-viewer "cat > /C/luado/JavaInstallation/0x%k.%t" --list-keys 29BEA2A645F2D6CED7FB12E02B172E3E156466E8
```

# Conclusion

The command above should produce a picture if there is one associated with the key, and then you can google the name of the person associated with the registered key and check online on LinkeIn, facebook, instagram, if said person does match the picture provided.

Keep in mind the picture can still be fake. The only real way to check for sure the key is to get in touch personally with said person, and have it check via phone call or in person that the given public key was really issued by her.
