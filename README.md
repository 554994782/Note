# Note

podspec验证
pod lib lint Mall.podspec --allow-warnings
pod spec lint Mall.podspec --allow-warnings
pod repo push ESSpecs Mall.podspec --allow-warnings
公开
pod trunk push TFDropDownMenu.podspec --allow-warnings

创建jenkins
brew install jenkins
sudo gem install fastlane
brew install jq

脚本
IPANAME="consumer"
EXPORTPLIST='/Users/jiangyunfeng/Desktop/ESHome/ExportOptions_QA.plist'
#EXPORTPLIST='/Users/shejijia/workspace/ExportOptions_AppStore.plist'

#React Native
export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
cd ./reactnative
npm install
cd ..

#Build & Archive
export PATH="$HOME/.fastlane/bin:$PATH"
security unlock-keychain -p jiang ~/Library/Keychains/login.keychain
fastlane gym --workspace "shejijia.xcworkspace" --scheme "Consumer" --clean --configuration "$PACKAGE_TYPE" --output_directory "./build" --export_options ${EXPORTPLIST} --output_name ${IPANAME}

#Upload to Pgy
cd ./Build
result=`curl -F "file=@${IPANAME}.ipa" -F "uKey=0c91f225b5229ebaad108bb2acc910da" -F "_api_key=bf32bedb5cfe74b375e751d959182e2a" https://qiniu-storage.pgyer.com/apiv1/app/upload`
echo $result
url=`echo $result | jq '.["data"]["appQRCodeURL"]' | sed 's/\"//g'`
curl $url -o ${IPANAME}.png
