---
tags: terraform, azure
---

Configuration fo b2c tenant and app registration:

```
resource "azurerm_aadb2c_directory" "modeorbit_sso" {
	country_code = var.country_code
	data_residency_location = var.data_residency_location
	display_name = var.display_name
	domain_name = var.domain_name
	resource_group_name = var.resource_group_name
	sku_name = var.sku_name
	tags = var.tags
}

provider "azuread" {
	tenant_id = azurerm_aadb2c_directory.modeorbit_sso.tenant_id
}


resource "azuread_application" "web_app" {
	display_name = "web-app"
	identifier_uris = []
	group_membership_claims = []
	device_only_auth_enabled = false
	fallback_public_client_enabled = false
	oauth2_post_response_required = false
	prevent_duplicate_names = true
	sign_in_audience = "AzureADandPersonalMicrosoftAccount"

	api {
		known_client_applications = []
		mapped_claims_enabled = false
		requested_access_token_version = 2
	}



	# API permissions
	required_resource_access {
		resource_app_id = "00000003-0000-0000-c000-000000000000"
		resource_access {
			id = "37f7f235-527c-4136-accd-4a02d197296e"
			type = "Scope"
		}

		resource_access {
			id = "7427e0e9-2fba-42fe-b0c0-848c9e6a8182"
			type = "Scope"
		}
	}

	single_page_application {
		redirect_uris = [
			"https://jwt.ms/",
		]
	}

	web {
		implicit_grant {
			access_token_issuance_enabled = true
			id_token_issuance_enabled = true
		}
	}
}
```
