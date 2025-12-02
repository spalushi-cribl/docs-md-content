# Securing the Auth Token


## Securing the Leader's Auth Token

Cribl **strongly recommends** that when you first install Cribl Stream, you change the Leader's auth token value from the default `criblmaster` to a unique secure value.

This applies whether you configure your Leader via the UI, or via a Docker Compose file.

### Changing the Default Auth Token Value 

Create a unique, strongly secure token value. Cribl recommends creating a string that contains at least 14 characters and includes uppercase letters, lowercase letters, and numbers.

One option is to generate a token value with a shell command. For example, on Linux or Mac OS, the following command generates a 32-character value:
  ```shell
  head /dev/urandom | LC_ALL=C tr -dc A-Za-z0-9 | head -c32 | cut -c 1-
  ```

Once you have created a secure auth token value, paste it into the **Auth token** field in the UI, or in the appropriate place in your Docker Compose file (if applicable).

In the UI, you'll find the **Auth token** setting under **Global Settings** > **System** > **Distributed Settings** > **Leader Settings**, as shown in the screenshot below.

![Distributed > Leader Settings](leader-settings.eb34f1c547.png)
{border="true"}
