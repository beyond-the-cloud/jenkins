helm lint ./notifier-webapp

git remote rm origin
git remote add origin https://beyond-the-cloud:${GITHUB_TOKEN}@github.com/beyond-the-cloud/notifier-webapp-helm-chart.git
git symbolic-ref HEAD refs/heads/master

git status
git log --oneline

git pull origin master

git status
git log --oneline

git tag
export VERSION=$(git describe --abbrev=0 --tags)
echo $VERSION

export a=$(echo "$VERSION" | cut -d. -f1)
export b=$(echo "$VERSION" | cut -d. -f2)
export c=$(echo "$VERSION" | cut -d. -f3)
export NEW_VERSION="${a}.${b}.$(($c + 1))"
echo $NEW_VERSION

sed -i -e "s/^version:.*/version: ${NEW_VERSION}/g" notifier-webapp/Chart.yaml

zip -r notifier-webapp-v${NEW_VERSION}.zip notifier-webapp

git reset --hard HEAD
git status

npx release-it patch --ci --no-git.requireUpstream --no-git.requireCleanWorkingDir --github.release --github.releaseName="notifier-webapp-v${NEW_VERSION}" --github.assets=notifier-webapp-v${NEW_VERSION}.zip