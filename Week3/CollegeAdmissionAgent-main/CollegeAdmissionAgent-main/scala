import sttp.client4.quick.*
import scala.util.{Success, Failure}
import java.util.Base64
import java.nio.charset.StandardCharsets
import play.api.libs.json._

@main def main() = {
	// NOTE: you must manually set API_KEY below using information retrieved from your IBM Cloud account (https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml-authentication.html?context=wx)

	val API_KEY = "<your API key>"

	// Get IAM service token
	val iam_url = uri"https://iam.cloud.ibm.com/identity/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$API_KEY"
	val iam_response = quickRequest.post(iam_url).header("Content-Type", "application/x-www-form-urlencoded").header("Accept",
		"application/json").send()
	val iamtoken_json: JsValue = Json.parse(iam_response.body)

	val iamtoken = (iamtoken_json \ "access_token").asOpt[String] match {
		case Some(x) => x
		case None => ""
	}

	// TODO:  manually define and pass list of values to be scored
	val payload_scoring: JsValue = Json.parse("""{"messages":[{"content":"","role":""}]}""")

	val scoring_url = uri"https://us-south.ml.cloud.ibm.com/ml/v4/deployments/499d3562-0446-4957-a1c0-67822a11e26f/ai_service_stream?version=2021-05-01"

	val response_scoring = quickRequest.post(scoring_url).body(payload_scoring).header("Content-Type",
		"application/json").header("Authorization", "Bearer " + iamtoken).send()
	println("scoring response")
	println(response_scoring.body)
}
