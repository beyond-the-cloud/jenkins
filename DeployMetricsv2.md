```
if [ -z '$(ls -A)' ]; then
  rm -r *
fi

gcloud config configurations activate default

gcloud auth activate-service-account jenkins@corded-terrain-309700.iam.gserviceaccount.com --key-file="$JING_GCLOUD_CREDENTIALS"

export VAR="$(gcloud container clusters list)"

if [ "$VAR" = "" ]; then
    echo "NO CLUSTER"
else
    gcloud container clusters get-credentials corded-terrain-309700-gke --region us-east1
    
    # Define variables
    echo "---------------------------------------------------------------------"
    echo "Define variables"
    echo "---------------------------------------------------------------------"
    
    owner="beyond-the-cloud"
    repo="helm-chart-metrics"
    GITHUB_API_TOKEN=${GITHUB_TOKEN}
    GH_API="https://api.github.com"
    GH_REPO="$GH_API/repos/$owner/$repo"
    GH_LATEST="$GH_REPO/releases/latest"
    AUTH="Authorization: token $GITHUB_API_TOKEN"
    
    # Read asset name and id
    echo "---------------------------------------------------------------------"
    echo "Read asset name and id"
    echo "---------------------------------------------------------------------"
    
    response=$(curl -sH "$AUTH" $GH_LATEST)
    id=`printf '%s' "$response" | jq '.assets[0] .id' |  tr -d '"'`
    name=`printf '%s' "$response" | jq '.assets[0] .name' |  tr -d '"'`
    GH_ASSET="$GH_REPO/releases/assets/$id"
    
    # Print Details
    echo "---------------------------------------------------------------------"
    echo "Print Details"
    echo "Assets Id: $id"
    echo "Name: $name"
    echo "Assets URL: $GH_ASSET"
    echo "---------------------------------------------------------------------"
    
    # Downloading asset file
    echo "---------------------------------------------------------------------"
    echo "Downloading asset file"
    echo "---------------------------------------------------------------------"
    
    curl -L -o "$name" -H "$AUTH" -H 'Accept: application/octet-stream' "$GH_ASSET"
    
    ls -al
    
    unzip -o "$name"
    
    ls -al
    
    export CHART_VERSION=$(curl -H "Authorization: token ${GITHUB_TOKEN}" "https://api.github.com/repos/beyond-the-cloud/helm-chart-metrics/releases/latest" | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
    echo "CHART_VERSION: ${CHART_VERSION}"
    
    export VAR="$(helm ls | grep prometheus)"
    
    if [ "$VAR" = "" ]; then
        echo "NO RELEASE"
        helm lint metrics
        helm dependency update ./metrics
        helm install prometheus ./metrics
    else
        echo "RELEASE FOUND"
        helm lint metrics
        helm dependency update ./metrics
        helm upgrade prometheus ./metrics
    fi
    
    rm -rf *
fi

if [ -z '$(ls -A)' ]; then
  rm -r *
fi
```
```
gcloud config list

gcloud config configurations activate default

gcloud config configurations delete xzhang-gcloud --quiet

gcloud config list

gcloud config configurations create xzhang-gcloud

gcloud config set project xzhang-csye7125-term-proj

gcloud config set account zhang.xinyu3@northeastern.edu

gcloud auth activate-service-account jenkins-gke@xzhang-csye7125-term-proj.iam.gserviceaccount.com --key-file="$XZHANG_JENKINS_GKE_SA_KEY"

export VAR="$(gcloud container clusters list)"

if [ "$VAR" = "" ]; then
    echo "NO CLUSTER"
else
    gcloud container clusters get-credentials xzhang-csye7125-term-proj-gke --region us-east1
    
    # Define variables
    echo "---------------------------------------------------------------------"
    echo "Define variables"
    echo "---------------------------------------------------------------------"
    
    owner="beyond-the-cloud"
    repo="helm-chart-metrics"
    GITHUB_API_TOKEN=${GITHUB_TOKEN}
    GH_API="https://api.github.com"
    GH_REPO="$GH_API/repos/$owner/$repo"
    GH_LATEST="$GH_REPO/releases/latest"
    AUTH="Authorization: token $GITHUB_API_TOKEN"
    
    # Read asset name and id
    echo "---------------------------------------------------------------------"
    echo "Read asset name and id"
    echo "---------------------------------------------------------------------"
    
    response=$(curl -sH "$AUTH" $GH_LATEST)
    id=`printf '%s' "$response" | jq '.assets[0] .id' |  tr -d '"'`
    name=`printf '%s' "$response" | jq '.assets[0] .name' |  tr -d '"'`
    GH_ASSET="$GH_REPO/releases/assets/$id"
    
    # Print Details
    echo "---------------------------------------------------------------------"
    echo "Print Details"
    echo "Assets Id: $id"
    echo "Name: $name"
    echo "Assets URL: $GH_ASSET"
    echo "---------------------------------------------------------------------"
    
    # Downloading asset file
    echo "---------------------------------------------------------------------"
    echo "Downloading asset file"
    echo "---------------------------------------------------------------------"
    
    curl -L -o "$name" -H "$AUTH" -H 'Accept: application/octet-stream' "$GH_ASSET"
    
    ls -al
    
    unzip -o "$name"
    
    ls -al
    
    export CHART_VERSION=$(curl -H "Authorization: token ${GITHUB_TOKEN}" "https://api.github.com/repos/beyond-the-cloud/helm-chart-metrics/releases/latest" | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
    echo "CHART_VERSION: ${CHART_VERSION}"
    
    export VAR="$(helm ls | grep prometheus)"
    
    if [ "$VAR" = "" ]; then
        echo "NO RELEASE"
        helm lint metrics
        helm dependency update ./metrics
        helm install prometheus ./metrics
    else
        echo "RELEASE FOUND"
        helm lint metrics
        helm dependency update ./metrics
        helm upgrade prometheus ./metrics
    fi
    
    rm -rf *
fi
```