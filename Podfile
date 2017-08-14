use_frameworks!

def socket
  pod 'CocoaAsyncSocket', '~> 7.4.3'
end

target 'PacketProcessor_macOS' do
  platform :osx, '10.10'
  socket
end

target 'PacketProcessor_iOS' do
  platform :ios, '9.0'
  socket
end

post_install do |installer|
    installer.pods_project.targets.each do |target|
        target.build_configurations.each do |config|
            config.build_settings['APPLICATION_EXTENSION_API_ONLY'] = 'YES'
            config.build_settings['ONLY_ACTIVE_ARCH'] = 'NO'
        end
    end
end
