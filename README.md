const stripe = require('stripe')(process.env.STRIPE_SK);
exports.handler = async (e)=>{
  const {items} = JSON.parse(e.body);
  const session = await stripe.checkout.sessions.create({
    payment_method_types:['card'],
    line_items:items,
    mode:'payment',
    success_url:`${process.env.URL}/?success`,
    cancel_url:`${process.env.URL}/`
  });
  return {statusCode:200,body:JSON.stringify({id:session.id})};
};
