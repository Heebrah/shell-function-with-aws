# shell-function-with-aws

When using AWS CLI or SDKs, **environment variables** are a common way to configure your AWS credentials and settings. Here's a summary of key environment variables used by AWS:

---

### 🔐 **Credential-related environment variables:**

| Variable                | Description                                                                      |
| ----------------------- | -------------------------------------------------------------------------------- |
| `AWS_ACCESS_KEY_ID`     | Your AWS access key ID                                                           |
| `AWS_SECRET_ACCESS_KEY` | Your AWS secret access key                                                       |
| `AWS_SESSION_TOKEN`     | (Optional) Temporary session token, used with IAM roles or STS                   |
| `AWS_PROFILE`           | Specifies the named profile to use from `~/.aws/credentials` and `~/.aws/config` |

---

### 🌐 **Configuration-related environment variables:**

| Variable                      | Description                                                                |
| ----------------------------- | -------------------------------------------------------------------------- |
| `AWS_DEFAULT_REGION`          | Default region (e.g., `us-east-1`)                                         |
| `AWS_REGION`                  | Sometimes used interchangeably with `AWS_DEFAULT_REGION`                   |
| `AWS_DEFAULT_OUTPUT`          | Output format (e.g., `json`, `text`, `table`)                              |
| `AWS_CONFIG_FILE`             | Custom location of the config file (defaults to `~/.aws/config`)           |
| `AWS_SHARED_CREDENTIALS_FILE` | Custom location of the credentials file (defaults to `~/.aws/credentials`) |

---

### 📌 Notes:

* **Environment variables override** settings in config and credentials files.
* When using **temporary credentials** (like from `aws sts assume-role`), you must set `AWS_SESSION_TOKEN`.
* You can set them in your shell like so:

  ```bash
  export AWS_ACCESS_KEY_ID="your-access-key"
  export AWS_SECRET_ACCESS_KEY="your-secret-key"
  export AWS_DEFAULT_REGION="us-west-2"
  ```


  To **verify AWS configuration** using functions in a shell script, you can add checks that call AWS CLI commands to confirm key aspects like credentials, region, profile, or even connectivity. Here's an extended and practical example with functions:

---

### ✅ **Shell Script with AWS Verification Functions**

```bash
#!/bin/bash

# Function: Check if AWS CLI is installed
check_aws_cli() {
    if ! command -v aws &> /dev/null; then
        echo "❌ AWS CLI is not installed."
        return 1
    fi
    echo "✅ AWS CLI is installed."
}

# Function: Check if AWS profile is set
check_aws_profile() {
    if [ -z "$AWS_PROFILE" ]; then
        echo "❌ AWS_PROFILE is not set."
        return 1
    fi
    echo "✅ AWS_PROFILE is set to '$AWS_PROFILE'."
}

# Function: Check if AWS region is set
check_aws_region() {
    if [ -z "$AWS_REGION" ] && [ -z "$AWS_DEFAULT_REGION" ]; then
        echo "❌ AWS region is not set."
        return 1
    fi
    REGION=${AWS_REGION:-$AWS_DEFAULT_REGION}
    echo "✅ AWS region is set to '$REGION'."
}

# Function: Verify AWS credentials are working
verify_aws_credentials() {
    if ! aws sts get-caller-identity --output text &> /dev/null; then
        echo "❌ AWS credentials are not valid or have expired."
        return 1
    fi
    echo "✅ AWS credentials are valid."
}

# Run all checks
check_aws_cli || exit 1
check_aws_profile || exit 2
check_aws_region || exit 3
verify_aws_credentials || exit 4
```

---

### 🔍 What This Script Verifies:

* AWS CLI is installed
* `AWS_PROFILE` is set
* Region is set via `AWS_REGION` or `AWS_DEFAULT_REGION`
* Valid credentials by making an actual call to AWS (`sts get-caller-identity`)


# Project on Verification of AWS account

1. 


````
#!/bin/bash
run_environment_script(){
    # Check for exactly one argument
        if [ "$#" -ne 1 ]; then
                echo "Usage: $0 <environment>"
                        exit 1
           fi

           # Access the first argument
               ENVIRONMENT=$1

                   # Acting based on the argument value
       if [ "$ENVIRONMENT" = "local" ]; then
             echo "Running script for Local Environment..."
             echo "Starting local services..."

 elif [ "$ENVIRONMENT" = "testing" ]; then
         echo "Running script for Testing Environment..."
                 echo "Running test suite..."

                     elif [ "$ENVIRONMENT" = "production" ]; then
                             echo "Running script for Production Environment..."
                                     echo "Deploying to production..."

                         else
                               echo "Invalid environment specified. Please use 'local', 'testing', or 'production'."
                                         exit 2
                                             fi
                                             }
        # Call the function with all passed arguments
        run_environment_script "$@"

```
https://imgur.com/Z8cWFg4
