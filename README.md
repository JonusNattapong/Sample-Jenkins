# Sample-Jenkins

Build Bloom is a small static website deployed by Jenkins.

The `Jenkinsfile` validates the page and copies it to the dedicated Nginx document root for this project on the Windows server.

Jenkins deploys to `C:\Users\pts\sample-jenkins\dist\index.html`.

Open the deployed site at `http://192.168.88.178/sample-jenkins/`.
