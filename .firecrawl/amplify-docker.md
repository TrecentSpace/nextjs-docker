![Thumbnail (1920x1080)](https://i.ytimg.com/vi/rA5l82vypXc/maxresdefault.jpg)
# [Next.js on Fargate - Serverless Container Hosting with Docker and AWS Amplify](https://www.youtube.com/watch?v=rA5l82vypXc)

**Visibility**: Public
**Uploaded by**: [Nader Dabit](https://www.youtube.com/@naderdabit)
**Uploaded at**: 2021-01-12
**Published at**: 
**Length**: 12:28
**Views**: 6603
**Likes**: 151
**Category**: Entertainment

## Description

```
In this video we'll start from scratch, creating a new Next.js app and then initializing a new Amplify project in the Next.js app directory. We'll then configure a custom domain and deploy the Next.js app to Amazon ECS on AWS Fargate using the Amplify CLI using the custom domain.

Delete all infrastructure at any time by running "amplify delete".

0:00 – Introduction
1:00 – Initializing the project
2:18 – Configuring the custom domain
4:33 – Enabling Fargate Hosting with the Amplify CLI
5:46 – Configuring the Dockerfile
7:27 – Testing the Docker image locally
8:32 – Deploying the app to Fargate using the Amplify CLI
12:00 – Conclusion

Dockerfile: https://gist.github.com/dabit3/6eb125dad05c1b1723bc44b6618e8ac4
Amplify Container docs: https://docs.amplify.aws/cli/usage/containers#hosting
Blog post: https://dev.to/dabit3/serverless-containers-with-next-js-aws-fargate-and-aws-amplify-17fe
```

## Transcript

so one of the most recent announcements
from aws amplify was the
announcement for container support using
fargate with aws amplify
now there's a couple of ways that you
can use containers with amplify now
one is for deploying an api so we have
an option for deploying a rest api
as well as a graphql api but we also
have container support for hosting
meaning you can essentially just take a
docker image and do whatever you'd like
with it
and deploy it using the amplify cli and
this will coordinate everything with
fargate and ecs and everything you need
to kind of get up and running
using a custom domain now this is what
we're going to do in this video we're
going to build
from scratch a brand new next app we're
going to initialize amplify hosting with
fargate using docker
and then we're going to deploy our app
to aws
in fargate and then we're going to make
an update and kind of deploy an update
to kind of see how
that process looks i'm going to do so
using a custom domain
so i hope you enjoy this video and we're
going to go ahead and get started
right away so the first thing that i'm
going to do is i'm going to go ahead and
create a new
next js app using npx
and once this is created we're going to
then go ahead and initialize
a new amplify project
all right so i'm going to go ahead and
change into the new directory
and we're going to run amplify init
and for these choices um it doesn't
really matter a whole lot because we're
not going to be using these commands
we're going to be using our docker file
but since we're using um you know
next.js we might go ahead and choose our
source directory as the root directory
and the distribution directory as dot
next
and then we can kind of take the rest of
those defaults
and then finally we're going to be
prompted for our
aws profile and i'll just choose my
default profile
all right so now we have an amplified
project initialized
so the next thing we need to do is we
want to be using a custom domain so i
want to kind of deploy this on my own
url
so what i'm going to do is i'm going to
go to google domains
where i have purchased this domain name
next.js on fargate and this is what
we're going to be using to deploy our
custom domain
and what we're going to basically be
needing to do is updating our dns to
kind of match the name servers that
we're going to create in aws
so the next thing after that we have our
domain this could be godaddy it could be
really anywhere including route53 if you
want to do that
once you have bought your domain and you
have the ability to change the dns we're
going to now configure this within route
53
so i'm going to go ahead and search for
route 53
and this is going to bring us to the
dashboard where we can click on hosted
zones
and we're going to click create hosted
zone and this is essentially going to
create
a place for us to configure our new
domain giving us the name servers that
we're going to need
so for our domain it's going to be
next.js on fargate.com
and we can just keep all the default
settings i'm going to go ahead and click
create hosted zone
so this is going to now give us our name
servers that we can now configure
for our dns and we're pretty much done
at this point
working with the aws dashboard so i'm
going to click on use custom name
servers
i'm just going to start dropping these
four name servers into my dns
settings
so now we're going to save we're going
to click change name servers anyway
and this is going to take you know some
time to propagate but we're going to
kind of
step away and come back to this and then
hopefully by the time we get back
everything has been propagated and all
that
but we can go ahead and use this domain
because once it's configured in route 53
and we've moved over our our name
servers over to our hosting provider we
can start using
this domain name so now we can go back
to
the amplify console here i'm sorry we
can go back to our
our cli here and what we're going to now
do is run
amplify configure
project because we want to now configure
our project to
be aware that we're interested in using
containers so we can basically just
you know take all of the default
settings that we have here
but we're going to now have a new prompt
that's going to say
something like do you want to enable
container-based deployments we can
choose yes
and then we can keep our existing aws
profile that we configured earlier
and then now we can run amplify ad
hosting
and we'll see that we have the ability
to choose
based hosting with aws fargate so what
i'm going to do is choose that
and then i'm going to choose my domain
name as www.nexjsonfargate.com
and do i want to con uh automatically
protect my domain with uh cognito
uh do i want any authentication
essentially i don't i don't want that
for this project so i'm going to choose
no
okay so now that we've set all that up
let's go ahead and open up the project
in our text editor
and what you should now see is
a docker file and in this docker file
this is where we're going to basically
go ahead and configure our deployment
now if you've worked with
docker before you know that you pull
node.js from
you know a different image location but
what we're going to basically be using
here
is ecr and ecr allows you to kind of
pull from a local
registry run by aws and the docker image
that we're going to be working with
is something i'm going to paste into the
notes of this tutorial
and it's basically a really really
concise docker file for deploying
nexjs now this probably isn't the most
you know efficient or um there's some
you know probably some caching and
things like that you can do to kind of
make this more i would say production
ready but this actually will work fine
and it's nothing wrong with the kind of
starting from this because it is a lot
more simple and there's only a few
commands
and this is a good way for us to kind of
just get docker
you know up and running with next.js and
this is what we're going to use
so i'm just going to kind of uh remove
what's there and we're going to kind of
go through this docker file
we're going to be pulling um you know
node.js from this location
we're going to set our work directory
we're going to copy package
json and yarn.lock if they exist and
we're going to go ahead and then
run yarn to install our node modules
we're going to then
copy the rest of the files from our
project into
the app directory and then we're going
to go ahead and build the app and we're
going to expose
the port of 3000 and then we're going to
run yarn start
so from this we should be you know this
is all
that we need to do really we should be
able to go ahead and deploy this
but let's say we wanted to go ahead and
run this locally we can do that so
basically what i'm going to do is i'm
going to say
docker build and then
we're going to give it a tag of like
next app
and then once this is built we'll go
ahead and test this out locally just to
kind of make sure
this all looks like it's working
properly
okay so after the build is complete we
should be able to run
docker image ls and see that we have
this
next app docker image and then we can
now say docker run
set the port as something like 8080 and
then we know that the
app that we're running is going to be
running on 3000
and then we'll just pass in the name of
our app so now we should be able to go
to localhost
8080 and if our image is you know
deployed
successfully locally we should be able
to kind of see it running
all right so we have our image running
locally now let's deploy this
and to do that i'm going to go ahead and
stop the
running instance locally and we're going
to run amplify
publish
amplify publish should now prompt us and
ask us if we are sure we want to
continue
we're going to go ahead and choose yes
and that's it now our app should be
being deployed
to vargate um and we can now jump into
the aws console
and after we wait a couple of minutes to
kind of see the build process happening
and we're going to go to code pipeline
and we should now see that we have this
build happening
and from here you can kind of see the
stages and
if there's anything that messes up you
know if there's any errors or anything
like that
um you can also go into the build
projects to actually see
some more fond grain details around
logging and stuff
so if you have any errors during the
build process
they should show up here so um with that
being said
what we're going to basically do now is
uh wait a few minutes
and kind of let this build complete and
then once it does
we're going to then go test out our live
domain
so i guess the thing that we're really
waiting waiting on to finish
here is uh the pipeline
so now that the pipeline has uh finished
we see that it's succeeded and this is
kind of um
meaning everything has been deployed and
we can now hopefully
test out our new application in a few
minutes once the dns
is completed propagating
so to test it out we'll go ahead and
open our web browser and we'll just go
to
nexjs on fargate.com or whatever domain
that you're using
and we should see that our app has been
deployed successfully
what about updates so what we might want
to do is go in and make
a change and kind of like deploy a new
version so the simplest way for me to
kind of
show you how this works is instead of
saying welcome to next.js
maybe we can say welcome to next.js like
version two
and then hopefully we can tell if this
deployment update has worked because if
so then our live domain will be updated
with
welcome to next.js version two so to
test this out what we're going to do
to kind of make sure that this is
working is we're going to go ahead
and go to our cli and we're going to now
whoops we're going to now run amplify
publish again
and once that's succeeded we should now
see
a message saying publish started you can
now check the status of your deployment
and it kind of gives us a link to check
that out whoops
and here if we go to pipelines
we see that a new pipeline has started
if we go to a build
we should also see a new build has
started so
what we're going to now do is wait a
couple of minutes for this deployment to
succeed and then we're going to refresh
and see if our update is there
all right so now that the build has
successfully completed we can now go to
our
url
and now we see that we have next.js
version 2 meaning our deployment has
successfully been updated and everything
is now working
so that's it so i hope you kind of
learned a little bit about how amplify
hosting
manages containers i'm going to be doing
a few more videos about this
subject like i mentioned we're going to
look at how to deploy
you know the regular ssr version of
react the new react server components
we're going to be doing apis with
containers as well so
look out for those videos and if you're
not already subscribed i would
appreciate if you subscribe to my
channel
and i will see you next time