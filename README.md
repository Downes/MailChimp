MailChimp 3.0 version 0.01
======================

This is the initial implentation of the MailChimp API version 3.0 in Perl.

It's pretty basic, but allows you to create a campaign, fill it with content,
and sent it to a list. It expects templates and lists to have already been
created on MailChimp.

Instead of using somethinbg big and heavy to implement, like Plack or Moose,
this module is built using Perl Curl. 

INSTALLATION

To install this module type the following:

   perl Makefile.PL
   make
   make test
   make install

DEPENDENCIES

This module requires these other modules and libraries:

```
  local::lib; # sets up a local lib at ~/perl5
  WWW::Curl::Easy;
  JSON;
  Digest::MD5;
```

local::lib allows implementation on Reclaim and probably not otherwise needed,
and if it breaks your implementation you can comment it out.

Digest::MD5 is needed to add emails to the mailing lint, but since I haven't implemented
that, is isn't required either.


=head1 NAME

MailChimp - Interface for MailChimp API 3.0

See https://mailchimp.com/developer/marketing/api/

=head1 SYNOPSIS

```
use MailChimp;

my $account = MailChimp->new({
		username => 'username email',
		datacenter => 'us18',
		version => '3.0',
		url => 'api.mailchimp.com',
		apikey => 'apikey'
});

$account->campaigns();      # List campaigns, etc
$account->templates();
$account->lists();

my $listid = "375520a149";  # Can also be defined using API but I haven't built that in yet
my $template_id = 69461;

	my $campaign = MailChimp::Campaigns->new({
		account => $account,
		type => 'regular',
		recipients => {
		    list_id => $listid
	    },
		settings => {
		    subject_line => 'Test',
		    preview_text => 'Text',
		    title => '',
		    from_name => 'moi',
		    reply_to => $account->{username},
		    to_name => "",
		    folder_id => "",
		    auto_fb_post => [],
		    template_id => $template_id
		},
		content_type => 'template'
	});
  
	$campaign->create();    # I prefer to keep this as an extra step 

	print $campaign->to_string();

  $campaign->add_content({
		html => "Some test HTML content",
		plain_text => "Some test text content"
	});

	$campaign->send();

```
=head1 DESCRIPTION

Implements a basic campaign creation and send topredefined email list in MailChimp.
Created by Stephen Downes to support the gRSShopper project.

=head2 EXPORT

None by default.

=head1 SEE ALSO

https://grsshopper.downes.ca

=head1 AUTHOR

Stephen Downes <lt>stephen@downes.ca<gt>

=head1 COPYRIGHT AND LICENSE

Copyright (C) 2021 by Stephen Downes

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself, either Perl version 5.10.1 or,
at your option, any later version of Perl 5 you may have available.


=cut



