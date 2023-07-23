# Temp Core Image
FROM mcr.microsoft.com/windows/servercore:ltsc2019 as core

ENV RUBY_VERSION 2.2.4
ENV DEVKIT_VERSION 4.7.2
ENV DEVKIT_BUILD 20130224-1432

RUN mkdir C:\\tmp
ADD https://github.com/oneclick/rubyinstaller/releases/download/ruby-${RUBY_VERSION}/rubyinstaller-${RUBY_VERSION}-x64.exe C:\\tmp
RUN C:\\tmp\\rubyinstaller-%RUBY_VERSION%-x64.exe /silent /dir="C:\Ruby_%RUBY_VERSION%_x64" /tasks="assocfiles,modpath"
ADD https://github.com/oneclick/rubyinstaller/releases/download/devkit-${DEVKIT_VERSION}/DevKit-mingw64-64-${DEVKIT_VERSION}-${DEVKIT_BUILD}-sfx.exe C:\\tmp
RUN C:\\tmp\\DevKit-mingw64-64-%DEVKIT_VERSION%-%DEVKIT_BUILD%-sfx.exe -o"C:\DevKit" -y

# Final Nano Image
FROM mcr.microsoft.com/windows/servercore:ltsc2019 as nano

ENV RUBY_VERSION 2.2.4
ENV RUBYGEMS_VERSION 2.6.13
ENV BUNDLER_VERSION 1.15.4



# COPY --from=core /Windows/System32/netapi32.dll /Windows/System32/netapi32.dll
COPY --from=core C:\\Ruby_${RUBY_VERSION}_x64 C:\\Ruby_${RUBY_VERSION}_x64
COPY --from=core C:\\DevKit C:\\DevKit

# RUN set PATH %PATH%;C:\DevKit\bin;C:\Ruby_%RUBY_VERSION%_x64\bin -m
ENV Path="C:\\Windows\\system32;C:\\Windows;C:\\DevKit\\bin;C:\\Ruby_${RUBY_VERSION}_x64\\bin"

RUN ruby C:\\DevKit\\dk.rb init
RUN echo - C:\\Ruby_%RUBY_VERSION%_x64 > C:\\config.yml
RUN ruby C:\\DevKit\\dk.rb install

# COPY gg.rb gg.rb
# RUN ruby gg.rb
# COPY rubygems-update-2.6.13.gem rubygems-update-2.6.13.gem
# RUN ruby -ropenssl -e 'p OpenSSL::X509::DEFAULT_CERT_FILE'

# COPY rubygems-2.6.13 /tmp/rubygems-2.6.13
# RUN cd /tmp/rubygems-2.6.13
# RUN ruby setup.rb


RUN mkdir C:\\tmp
ADD https://rubygems.org/gems/rubygems-update-2.6.13.gem C:\\tmp
RUN gem install --local C:\tmp\\rubygems-update-2.6.13.gem --no-ri --no-rdoc
RUN rmdir C:\\tmp /s /q

RUN ruby -ropenssl -e 'p OpenSSL::X509::DEFAULT_CERT_FILE'

RUN rvm -v

RUN gem update --system
RUN update_rubygems --no-ri --no-rdoc
RUN gem install bundler --version 1.15.4 --no-ri --no-rdoc

CMD [ "ping", "localhost", "-t" ]


# CMD [ "irb" ]