```
if [ -z '$(ls -A)' ]; then
  rm -r *
fi

gcloud config configurations activate default

gcloud auth activate-service-account 925083886688-compute@developer.gserviceaccount.com --key-file="$JING_GCLOUD_CREDENTIALS"

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
    repo="helmchart-app"
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
    
    export CHART_VERSION=$(curl -H "Authorization: token ${GITHUB_TOKEN}" "https://api.github.com/repos/beyond-the-cloud/helmchart-app/releases/latest" | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
    echo "CHART_VERSION: ${CHART_VERSION}"
    
    IP=$(gcloud sql instances list --filter=webapp --format="value(PRIMARY_ADDRESS)")
    echo $IP
    
    export VAR="$(helm ls | grep webapp | wc -l | sed -e 's/^[[:space:]]*//')"
    export VAR1="$(helm ls | grep notifier-webapp | wc -l | sed -e 's/^[[:space:]]*//')"
    export VAR2="$(helm ls | grep processor-webapp | wc -l | sed -e 's/^[[:space:]]*//')"
    
    if [ $VAR = 0 ]; then
            
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    elif [ "$VAR" = "1" ] && [ "$VAR1" = "1"] || [ "$VAR" = "1" ] && [ "$VAR2" = "1" ]; then
          
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    elif [ "$VAR" = "3" ] && [ "$VAR2" = "3" ]; then
          
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    elif [ "$VAR" = "4" ] && [ "$VAR1" = "1" ] && [ "$VAR2" = "3" ]; then
          
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    else
          
          echo "RELEASE FOUND"
          helm lint webapp
          helm upgrade --set mysql.dbHost=$IP webapp ./webapp
          
    fi
  	
    rm -r *
fi

if [ -z '$(ls -A)' ]; then
  rm -r *
fi
```
```

gcloud config list

gcloud config configurations activate default

gcloud config configurations delete xzhang-gcloud

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
    repo="helmchart-app"
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
    
    export CHART_VERSION=$(curl -H "Authorization: token ${GITHUB_TOKEN}" "https://api.github.com/repos/beyond-the-cloud/helmchart-app/releases/latest" | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
    echo "CHART_VERSION: ${CHART_VERSION}"
    
    IP=$(gcloud sql instances list --filter=webapp --format="value(PRIMARY_ADDRESS)")
    echo $IP
    
    export VAR="$(helm ls | grep webapp | wc -l | sed -e 's/^[[:space:]]*//')"
    export VAR1="$(helm ls | grep notifier-webapp | wc -l | sed -e 's/^[[:space:]]*//')"
    export VAR2="$(helm ls | grep processor-webapp | wc -l | sed -e 's/^[[:space:]]*//')"
    
    if [ $VAR = 0 ]; then
            
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    elif [ "$VAR" = "1" ] && [ "$VAR1" = "1"] || [ "$VAR" = "1" ] && [ "$VAR2" = "1" ]; then
          
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    elif [ "$VAR" = "3" ] && [ "$VAR2" = "3" ]; then
          
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    elif [ "$VAR" = "4" ] && [ "$VAR1" = "1" ] && [ "$VAR2" = "3" ]; then
          
          echo "NO RELEASE"
          helm lint webapp
          helm install --set mysql.dbHost=$IP webapp ./webapp
    
    else
          
          echo "RELEASE FOUND"
          helm lint webapp
          helm upgrade --set mysql.dbHost=$IP webapp ./webapp
          
    fi
  	
    rm -r *
fi
```